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