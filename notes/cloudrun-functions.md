# [Developing Applications with Cloud Functions on Google Cloud](https://www.cloudskillsboost.google/course_templates/505/video/361058)

# Introduction

- Serverless: You don't need to worry about patches, updating OS versions and other details related to dealing with machines.
- Auto Scalable
- Observablity.
- Pay as you go

Types: HTTP and Event-driven

- V2: Run time limit of up to 60 minutes for HTTP functions, and up to 10 minutes for event-driven functions to process longer request workloads

- When we deploy a zip code to the bucket, behind the stage, GCP uses cloud build to create a container of the function and deploy it to the cloud function.

- Manage traffic using revisions: When you deploy a new version of the function, it creates a new revision, and you can split the traffic between the revisions.

# Calling and connecting cloud functions

## Triggers

- HTTP
- Cloud Events
    - react to cloud events
    - event-driven functions
- multiple functions can be triggered by the same event
- A function cannot be bound to multiple triggers at the same time
- 2nd gen functions can be triggered by multiple resources, especially when using EventArc(125+ event sources)

## Workflow

- Is a fully-managed serverless orchestration platform
- Combines GCP services and APIs to create automated processes
- Each execution of a workflow is observable
- A workflow can hold state, retry, poll or wait for up to a year
- use YML to define the workflow

## Connecting to a VPC network

- A virtualized physical network that is implemented in a GCP production environment

Using serverless VPC access, you can connect your cloud functions to a VPC network, send requests and receive responses from the VPC network. And also, prevent requests and responses to and from internal resources from going over the internet

# Securing Cloud functions

## Securing access to cloud functions

- Identity-based controls
    - Validate the identity
    - Evaluate the permissions
- Network-based controls

Types of identities:

- Service accounts
- User accounts

For authentication Cloud functions use tokens:

- OAuth 2.0 Access tokens
- ID tokens (OICD)

## Protecting cloud functions

- CMEK: Customer-Managed Encryption Keys

Generate a private key that only you have access. When this functionality encrypts, the function source code, the container image, each instance of the function and data used in the event transport.

# Integration with Cloud Databases

## Connecting Cloud Functions To memorystory
## Env
## Firesore
## Secrets

# Best Practices

- Idempotency: Functions should be idempotent so they produce the same result when called multiple times
- HTTP Response: Always return a response to the client
- Avoid background activities: You don't have access to the CPU after the request is done, in that way, background activities can be a problem
- The same for temporary files, you don't have access to the file system after the request is done
- Avoid `uncaughtException`. These errors occur when an exception is thrown, but not properly handled by the code, the right thing to do is to return HTTP responses with the error code. This will avoid long-running functions and stick your cold start.
- Cloud Functions often re-cycles the execution environment of a previous invocation. If you declare a variable in global scope, its value can be reused in the subsequent invocations to a function.

# Resources

- [Hack: Use Cloud Functions as a webserver with Golang](https://medium.com/google-cloud/hack-use-cloud-functions-as-a-webserver-with-golang-42edc7935247)
- [organizing your cloud functions](https://codeburst.io/organizing-your-firebase-cloud-functions-67dc17b3b0da)
