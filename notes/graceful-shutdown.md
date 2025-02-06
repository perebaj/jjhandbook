# Graceful Shutdown/Upgrade

Problem: How do we kill something in a graceful way? Without losing data or causing downtime?

Independent of the language, framework, OS, cloud provider, if it's running on Docker or Kubernetes. All the systems have a way to handle this. **Signals**.

# Signals

Signals are a way to communicate with a process. They are used to tell a process to do something. For example, to stop, to reload, to restart, etc.

**SIGINT** and **SIGTERM** are Linux signals that request a process to terminate gracefully:

**SIGINT**
Usually initiated by the user, SIGINT is sent when the user presses the interrupt key combination, typically Ctrl+C.

**SIGTERM**
Can be sent by other processes or the system itself, SIGTERM is a generic signal used to cause program termination.

# Golang application

Usually, in a Go App, we create a context, that can be passed throught the application, including database connections, http server, etc.

If we want to stop the application, the right way is to cancel the context when we receive a signal that says that we need to stop the application.

# Cloud run Graceful Shutdown

> When a container instance is shut down on Cloud Run, a SIGTERM signal will be sent to the container and your application will have 10 seconds to exit. If the container does not exit by then, a SIGKILL signal (which you cannot capture) will be sent to abruptly close your application.

- https://cloud.google.com/blog/topics/developers-practitioners/graceful-shutdowns-cloud-run-deep-dive

- [Handle Signals within a golang server](https://github.com/GoogleCloudPlatform/golang-samples/blob/main/run/sigterm-handler/main.go)
