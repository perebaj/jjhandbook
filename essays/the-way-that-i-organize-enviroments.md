The way I like to organize my environments follows a few principles:

Preferably, the code should not know whether it's in production, staging, test, etc. So passing environment variables to my code, when those variables carry information like PROD, DEV, INTERNAL, is something I avoid as much as possible.

Personally, I find this kind of practice a problem, because it's not hard to end up with an environment where it's very confusing to manage and control what is going on. In my experience, the easiest way to manage a software is dealing with it like a black box, if that is or isn't some option to be turned on, this options should be controled via flags. https://prometheus.io/docs/prometheus/latest/feature_flags/

Take a look into the prometheus documentation, it's something like this.

Besides working for me, the method works for other software out there, because it's up to the engineer behind that software to decide what they want to enable or not according to the resources they have. The moment you put deployment environment details in your code, you are basically forcing a pattern that is exclusive to how you run that software at that exact moment.

Ok, but how do you achieve this in a minimally productive environment? The times I organized this, my projects were laid out like this.

Inside my cloud, I split the environments into different projects. Let's say:

Project-production
project-internal
project-development

When creating the infrastructure, I also avoid as much as possible adding prefixes or suffixes that refer to the name of my project. The infrastructure the 3 carry should always be the same, but sized differently depending on the needs my company has. With this, typos and implementation details between environments are drastically reduced: you have N different projects that, in theory, carry very similar configuration. The kustomize/helm/yaml each service uses is almost the same for all environments. What changes? In dev I want to use 500vCPU and in production 2vCPU... The configuration in production is more aggressive than in other environments.

But imagine having to touch terraform files like these:


resource "creating_of_s3_bucket" "name-of-bucket-production" {
  name = "name-of-bucket-production"
}

resource "creating_of_s3_bucket" "name-of-bucket-internal" {
  name = "name-of-bucket-internal"
}

resource "creating_of_s3_bucket" "name-of-bucket-development" {
  name = "name-of-bucket-development"
}

This is hell on earth. Imagine the number of typos that can happen in this process. The same goes for the code that will run in each environment.

Imagine getting to your code and seeing:

```go
if os.Getenv("ENV") == "production" {
  // code for production
} else if os.Getenv("ENV") == "internal" {
  // code for internal
} else if os.Getenv("ENV") == "development" {
  // code for development
}
```

It's much more practical to centralize all the feature flags for the functionality being maintained in a configuration file, and use that configuration throughout the code. That way, the decision is up to the user; they don't need to decide it based on the environment where the software is running.