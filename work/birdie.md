## Quarter 1 2023

- Push → Pull
- Pubsub emulator for local environment
- Custom Metrics - Stackdriver Adapter
    - HPA/VPA for optimize puller performance
- Uptime check for public APIS
    - Alert policy
        - Personal Notification channel
- Instrumentation/Metrics for puller using prometheus
- Structured Logs
- Handbook documentation
- Workload identity in Kubernetes cluster
- Reduce .yaml manifest files complexity. Using [overlay/base docs](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/#bases-and-overlays)
- Pubsub alert policy
    - failure_alert_oldest_unack_message
    - failure_alert_number_of_dead_letter_messages
- Elastic query history
- Dont treat exceptions in a generic way can cause disruption in code


> 👍 Push -> Pull
> 
> ## Issues with using push
> - Data consumption settings in *push* are implemented by default, so it's not possible to control the amount/velocity 🏃🏽‍♀️ 💨 of data that arrives at our services.
>   * We are susceptible to a self-DDoS attack 🧲
>   * *push* is preferred for applications that consume messages using serverless computing. 😶‍🌫️
>   * Impact on other services that are not related to data consumption. For example, search operations in the application.
> - *Push* subscriptions require routes exposed to the internet 🌐
>    * We have to manage the entire network infrastructure to perform this "exposure," including: Ingress, HTTPS Certificates & LoadBalancer 😮‍💨
> - Since we're constantly loading, enriching, and saving data, it's ideal to do fewer operations with more data to spare effort for our databases, rather than the opposite. Few operations with little data. 🤏🏽🤌🏽




> 🌻Handbook documentation
>
> "What if the most important part of "platform engineering" is maintaining a high quality wiki with proven, empathic patterns for Stream-aligned teams to follow?" - Matthew Skelton


> 🤡 Instrumentation/Metrics for a puller using Prometheus
> 
> This instrumentation involves simple metrics such as verifying the amount of time that each message takes to process and how many messages are processed per second. The difference that made an improvement in our metrics was choosing the right labels, such as:
> - Subscription name (to easily find the topic and see our status);
> - Delivery attempt (the last delivery attempt indicates that there are problems occurring);
> - If the message was ack or nack (status of messages);
> - Consumer type (which part of the code is processing this message?)


> 🫁 Pubsub alert policy
> 
> Alerts like:
> - alert policy to monitor the oldest unacknowledged message in the system.
> - alert policy to monitor the number of dead letter messages in the system.



> 👻 [Workload identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity) in Kubernetes cluster
> 
> Avoiding storing passwords in containers or saving them as plaintext in a Git repository is not a good practice. For this reason, when deploying on a Kubernetes cluster, GKE allows for the association of a service account with Kubernetes workloads without > > the need for messy configurations.

> 🥷 Elastic query history

> It can be a challenging task to identify problems when you have no idea how to find them. We have > encountered numerous issues with Elastic disruption, and creating mechanisms to trace expensive  
> queries in our system is an essential task, especially when it comes to Elastic search, which is > our primary database. Even when using a managed database, slow query tracking is not always a >default feature. That is why we have enabled the [slow query](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-slowlog.html#index-modules-slowlog) log to help us >identify and address these issues.
> 

> 😬 Dont treat exceptions in a generic way can cause disruption in your code
> 
> Errors make the process die, for this reason, it's essential to treat exceptions to exhibit the error and not finish the program. I figured out this problem because the database wasn't accepting requests, the code broke and didn't accept data at all. In python, just put in the main loop a try/except handler. Error fixed