# Coding with AI: local practices

Notes on the guardrails and habits I use locally when working with AI coding agents. The core rule is the same one from distributed systems: **enforce limits outside the model, because prompts are suggestions, not security boundaries.**

## Guardrails outside the model

Instructions in `AGENTS.md` or system prompts help steer behavior, but under long sessions or noisy tool outputs an agent can ignore or work around them. Anything that can break production or leak credentials needs a deterministic gate outside the LLM loop.

- **Pre-execution shell firewall ([`dcg`](https://github.com/Dicklesworthstone/destructive_command_guard))** — Wire `dcg` into `PreToolUse` hooks so destructive shell commands (`rm -rf`, `git reset --hard`, `git push --force`, `kubectl delete`, `terraform destroy`, `aws s3 rm`) are blocked before the process even spawns.
- **Opt in to your stack's packs** — By default `dcg` only enables `core.filesystem`, `core.git`, and `system.disk`. Cloud, Kubernetes, Terraform, database, storage, and secret-disclosure packs stay off until explicitly enabled in `~/.config/dcg/config.toml`.
- **Verify the hook matcher and test it live** — Different agent CLIs name their shell tool differently (for example, `run_command` vs `Bash`). A wrong matcher fails silently. After installing a hook, prove it actually fires using a harmless command that cannot touch real infrastructure (for example, `aws s3 rm s3://fake-bucket/x --recursive --endpoint-url http://127.0.0.1:1`).
- **Approve exceptions once, never globally** — When a blocked command is legitimately needed, use a one-shot override (`dcg allow-once <code>`) or run it manually instead of disabling the guard or widening permanent allowlists.
- **Production mutations require explicit approval** — Pair shell firewalls with explicit `deny` / `ask` permission rules for high-blast-radius commands (`terragrunt apply`, `kubectl -n prod`, `gsutil rm`) and production MCP servers so nothing mutates production without human confirmation.

## Secret hygiene

`.gitignore` stops Git from committing secrets; it does not stop an AI agent from reading them into its context window.

- **Block secret printing in the shell** — Enable secret-disclosure rules (such as `dcg`'s `secret_disclosure` pack) to block commands that print raw credentials to stdout (`aws secretsmanager get-secret-value`, `aws ssm get-parameter --with-decryption`, `op read`, `vault read`, `doppler secrets get`).
- **Inject secrets into processes, not transcripts** — Prefer runtime injection (`op run -- <cmd>`, `doppler run -- <cmd>`) and short-lived SSO sessions over plaintext `.env` files or long-lived static keys on disk.
- **Never commit or sync agent state blindly** — Keep local `.env` files, session transcripts, caches, and local settings overrides out of version control.

## Version-controlled agent setup

- **One private repo for global config** — Track global rules, hooks, custom agents, and skills in a private git repo using an allowlist `.gitignore` (ignore everything by default; allow only the config files you intend to sync).
- **Single source of truth across agents** — Symlink shared rules and skills across different coding-agent CLIs so conventions don't drift between tools.
- **Progressive disclosure over giant prompts** — Keep always-loaded global rules short and move multi-step workflows (deploys, incident triage, weekly checks) into on-demand skills so context stays focused.

## Workflow discipline

- **Small, single-purpose commits and PRs** — Split refactors, formatting, and behavior changes before committing or opening a PR. Review speed drops fast when an AI generates one sprawling diff.
- **Isolate parallel work with `git worktree`** — Run separate agent tasks in their own worktrees so parallel branches or experiments never collide with uncommitted work in the main checkout.
- **Triage automated review comments critically** — Verify every bot review comment against the current code before acting. Fix valid issues in isolated commits (one comment, one commit) and reply in-thread with either the fixing commit or the technical reason it was declined.
- **Verify with the real CI contract** — Run the repo's actual `Makefile` / build / test / vet targets after edits instead of hand-rolling commands that skip flags or build tags.

## Writing and documentation habits

- **Strip generated fluff** — Keep an explicit writing-style rule for commits, PRs, and docs: no restating the title, no ceremony headers over one-liners, no marketing adjectives ("robust", "seamless", "comprehensive").
- **Document the why, not volatile whats** — Capture decisions, constraints, and tradeoffs in docs and comments; avoid internal file paths, struct names, or line numbers that rot on the first refactor.
- **Explain changes with PCO** — For non-trivial problems or decisions, structure explanations around *Why* (impact), *How* (mechanism and tradeoffs), and *What* (done vs pending), defining jargon instead of assuming shared context.
