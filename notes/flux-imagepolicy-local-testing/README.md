# Validating Flux ImagePolicy / ImageRepository locally

A repeatable workflow for proving a Flux `ImagePolicy` matches the tag you
expect, before pushing the change to a real cluster. Uses a throwaway `kind`
cluster — zero contact with production, cleanup is one command.

## Why test ImagePolicy locally

Flux's image automation is silently fragile. If an `ImagePolicy` regex doesn't
match anything, or its `policy.numerical` field can't pick a winner, Flux
**stops updating the marker comment** in your HelmRelease and reports the
failure only in the resource status. Nothing crashes; deploys just quietly stop
flowing. You usually find out when a developer says *"my commit isn't reaching
dev"*.

Local validation lets you:

- prove a new regex actually matches the tag scheme in your registry,
- confirm `policy.numerical` (or `semver`) picks the tag you intended,
- demonstrate that adding a new policy alongside an existing one doesn't
  regress the old one.

## Prerequisites

- [`kind`](https://kind.sigs.k8s.io/) (any recent version)
- [`flux` CLI](https://fluxcd.io/flux/installation/)
- `kubectl`
- A way to mint a registry pull token. Examples below use `gh` for GHCR; any
  registry with a PAT flow works (Docker Hub, GCR, ECR, etc.).
- An image repository containing tags that match the pattern you want to test.

## Walkthrough

### 1. Install the Flux CLI

```sh
brew install fluxcd/tap/flux
```

### 2. Spin up a throwaway cluster

```sh
kind create cluster --name flux-test
```

### 3. Install only the controllers you need

A full `flux install` brings up source/kustomize/helm/notification/image
controllers. For ImagePolicy validation you only need three:

```sh
flux install --components=source-controller,image-reflector-controller,image-automation-controller
```

The image-reflector controller is the one that scans the registry and
evaluates `ImagePolicy`. The automation controller would normally write the
new tag back to git; for read-only validation you don't even need it, but
installing it lets you exercise the full flow if you want.

### 4. Get a registry pull token

For GHCR:

```sh
gh auth refresh -s read:packages
gh auth token > /tmp/ghcr-token
```

For other registries, mint a PAT with read scope on the repository you'll be
scanning and write it to `/tmp/<registry>-token`. The rest of the steps are
identical.

### 5. Create the pull secret in `flux-system`

The image-reflector controller runs in `flux-system` and resolves
`spec.secretRef` relative to that namespace.

```sh
kubectl create secret docker-registry ghcr-auth \
  --namespace flux-system \
  --docker-server=ghcr.io \
  --docker-username=<your-github-username> \
  --docker-password="$(cat /tmp/ghcr-token)"
```

### 6. Apply an ImageRepository plus one or more ImagePolicies

Always include a **control policy** — one you know already works in
production — alongside the new one you're validating. That way the test
proves both *"the new policy resolves correctly"* and *"the new policy
doesn't break the old one"* in the same run.

See the example YAML in the next section.

```sh
kubectl apply -f imagerepo.yaml
kubectl apply -f imagepolicies.yaml
```

### 7. Inspect

Give the reflector ~30 seconds to scan the registry, then:

```sh
flux get image repository
flux get image policy
```

Expected shape of healthy output:

```
NAME              LAST SCAN                   SUSPENDED  READY  MESSAGE
myapp             2026-06-08T13:42:11Z        False      True   successful scan: found 187 tags

NAME              LATEST IMAGE                                  READY  MESSAGE
myapp-semver      ghcr.io/example/myapp:1.4.2                   True   Latest image tag for 'ghcr.io/example/myapp' resolved to 1.4.2
myapp-dev         ghcr.io/example/myapp:dev-9f3b1c2-1717854321  True   Latest image tag for 'ghcr.io/example/myapp' resolved to dev-9f3b1c2-1717854321
```

If `READY` is `False`, the `MESSAGE` column tells you what failed (auth,
no matching tags, missing capture group, etc.). `kubectl describe
imagepolicy <name> -n flux-system` gives the full event stream.

### 8. Cleanup

```sh
kind delete cluster --name flux-test
rm -f /tmp/ghcr-token
```

## Example: timestamp-suffix dev tags with `policy.numerical`

A common pattern: dev builds tagged `dev-<sha>-<unix-ts>`. You want Flux to
pick the newest one by timestamp.

```yaml
# imagerepo.yaml
apiVersion: image.toolkit.fluxcd.io/v1beta2
kind: ImageRepository
metadata:
  name: myapp
  namespace: flux-system
spec:
  image: ghcr.io/example/myapp
  interval: 1m
  secretRef:
    name: ghcr-auth
---
# imagepolicies.yaml
apiVersion: image.toolkit.fluxcd.io/v1beta2
kind: ImagePolicy
metadata:
  name: myapp-semver  # control policy — known good
  namespace: flux-system
spec:
  imageRepositoryRef:
    name: myapp
  policy:
    semver:
      range: ">=1.0.0"
---
apiVersion: image.toolkit.fluxcd.io/v1beta2
kind: ImagePolicy
metadata:
  name: myapp-dev    # the one under test
  namespace: flux-system
spec:
  imageRepositoryRef:
    name: myapp
  filterTags:
    pattern: '^dev-[a-f0-9]+-(?P<ts>\d+)$'
    extract: '$ts'
  policy:
    numerical:
      order: asc
```

The named capture group `(?P<ts>\d+)` is what `extract` references; without
it, `policy.numerical` has nothing to compare and the policy reports
*"no latest image"*.

## Common pitfalls

- **`v1beta2 ImagePolicy is deprecated, upgrade to v1`.** Flux still accepts
  `v1beta2` and the behavior is identical, but plan to migrate the manifests
  in your real repo at some point. The local test works with either.
- **Token without `read:packages` (or equivalent).** `ImageRepository`
  status will show an authentication error and `LAST SCAN` stays empty.
- **Regex with no numerical capture group.** `policy.numerical` can't pick a
  winner; the `ImagePolicy` reports `no latest image`. Either add a `(?P<ts>...)`
  group and reference it via `extract`, or switch to `policy.semver`.
- **Forgetting to update the marker comment in the HelmRelease.** Flux
  matches the marker by `ImagePolicy` name. If you rename the policy or add
  a new one and don't update the `# {"$imagepolicy": "flux-system:<name>"}`
  comment next to the `tag:` field, Flux silently skips the marker — no
  error, just no update. Worth grepping for the old name when you rename.
- **Pull secret in the wrong namespace.** `secretRef` resolves inside the
  namespace where the `ImageRepository` lives. The reflector controller runs
  in `flux-system`, so the secret has to be there too.

## Why this is safe

- The `kind` cluster is local and isolated; no kubeconfig context for a real
  cluster is ever touched.
- The only outbound traffic is read-only registry scans against the image
  repo you point at.
- Cleanup is `kind delete cluster --name flux-test` plus removing the token
  file. Nothing persists.
