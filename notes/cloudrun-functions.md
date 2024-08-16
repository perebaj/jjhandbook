- [Hack: Use Cloud Functions as a webserver with Golang](https://medium.com/google-cloud/hack-use-cloud-functions-as-a-webserver-with-golang-42edc7935247)

## [Developing Applications with Cloud Functions on Google Cloud](https://www.cloudskillsboost.google/course_templates/505/video/361058)

### Introduction

- Serverless: You don't need to worry about patches, updating OS versions and other details related to dealing with machines.
- Auto Scalable
- Observablity.
- Pay as you go

Types: HTTP and Event-driven

- V2: Run time limit of up to 60 minutes for HTTP functions, and up to 10 minutes for event-driven functions to process longer request workloads

- When we deploy a zip code to the bucket, behind the stage, GCP uses cloud build to create a container of the function and deploy it to the cloud function.

### Best Practices

- Idempotency: Functions should be idempotent so they produce the same result when called multiple times
- HTTP Response: Always return a response to the client
- Avoid background activities: You don't have access to the CPU after the request is done, in that way, background activities can be a problem
- The same for temporary files, you don't have access to the file system after the request is done
- Avoid `uncaughtException`. These errors occur when an exception is thrown, but not properly handled by the code, the right thing to do is to return HTTP responses with the error code. This will avoid long-running functions and stick your cold start.
- Cloud Functions often re-cycles the execution environment of a previous invocation. If you declare a variable in global scope, its value can be reused in the subsequent invocations to a function.
