## Quarter 1 2023

- Push → Pull
- Custom Metrics - Stackdriver Adapter
    - HPA/VPA for optimize puller performance
- Uptime check for public APIS
    - Alert policy
        - Personal Notification channel
- Instrumentation/Metrics for puller using prometheus
- Structured Logs
- Handbook documentation


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