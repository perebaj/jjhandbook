# Fast search
- [Quarter 3 & Quarter 4 - 2022](#quarter-3--quarter-4---2022)
    - [Q3/Q4 - 2022 Resume](#q3q4---2022-resume)
- [Quarter 1 - 2023](#quarter-1-2023)
    - [Q1 Resume](#q1-2023-resume)
    - [Q1 Leasons](#leasons)
- [Quarter 2 - 2023](#quarter-2-2023)


# Quarter 1 & 2 - 2022
## Q1/Q2 - 2022 Resume
TODO

# Quarter 3 & Quarter 4 - 2022
## Q3/Q4 - 2022 Resume

In these two quarters, despite not keeping a daily documentation of my activities, I can vaguely remember the tasks that were done. Therefore, even though they are not detailed, I will try to summarize.

During this time, Birdie went through a drastic change process, and it was decided that the product we had was not enough to achieve the results that the management was expecting. As a result, a specific tech team was set up to start building this system. I can say that the main tech-focused objectives of this new product were:

- We would stop collecting public data from the internet and start receiving this data directly from customers.
- The data enrichment process should be as fast as possible (near-real-time pipeline).
- We would stop processing data via Batch and start communicating between teams and services via APIs using a data streaming bus.

Therefore, the two main sources of knowledge that we would start exploring were Streaming and APIs.

In this process, I can say that I was a protagonist in the discovery and implementation of both, even though I had never had contact with them. I hit my head a lot and made many mistakes that I now look back on and see as pathetic, haha.

In the end, we delivered the new product and were actually recognized for building something that seemed good. However, because we had a team with little experience in building distributed systems focused on APIs, many very important details were overlooked, details that are being unraveled in the summaries presented in 2023, [Quarter 1 - 2023](#quarter-1-2023) and [Quarter 2 - 2023](#quarter-2-2023)




# Quarter 1 2023

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
- Instrumentatate elastic/opensearch client to get performance evaluation for all queries 
- Getting ID token by impersonating a svc-account for local environment
- K8s alerts
    - CrashLoopBack -  👍🏽 
    - CPU usage is much higher than request 
    - Memory usage is at 90% regarding the memory limit
    - Volume/Disk usage of a pod is at 90%
- [bug] shadow pubsub timeout withoud nack or error message (difference between pubsub timeout x sdk library)
- Avoid build image all time. Just promote the most recent 🤔
- Histogram buckets with wise values can save your API metrics
- Cloud Armor
- Different APIs connect between trace-id into context logs. 
---

## TODO

- Structured Logs
- Custom Metrics - Stackdriver Adapter
- Uptime check for public APIS
- Hype guy frustations (https://twitter.com/jj_neno/status/1641464170796183556)
- k8s alerts
- [bug] shadow pubsub timeout withoud nack or error message (difference between pubsub timeout x sdk library)
- Avoid build image all time. Just promote the most recent 🤔
- Different APIs connect between trace-id into context logs. 
---


> 🕶 Pubsub emulator for local environment
>
> Have a local environment that it's trustworthy and looks like with your cloud environment is the pure fucking gold. Please spend more time doing that


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
> Avoiding storing passwords in containers or saving them as plaintext in a Git repository is not a good practice. For this reason, when deploying on a Kubernetes cluster, GKE allows for the association of a service account with Kubernetes workloads without the need for messy configurations.

> 🥷 Elastic query history
>
> It can be a challenging task to identify problems when you have no idea how to find them. We have > encountered numerous issues with Elastic disruption, and creating mechanisms to trace expensive  
> queries in our system is an essential task, especially when it comes to Elastic search, which is > our primary database. Even when using a managed database, slow query tracking is not always a >default feature. That is why we have enabled the [slow query](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-slowlog.html#index-modules-slowlog) log to help us >identify and address these issues.
> 


> 😬 Dont treat exceptions in a generic way can cause disruption in your code
> 
> Errors make the process die, for this reason, it's essential to treat exceptions to exhibit the error and not finish the program. I figured out this problem because the database wasn't accepting requests, the code broke and didn't accept data at all. In python, just put in the main loop a try/except handler. Error fixed


> 👣 Instrumentatate elastic/opensearch client to get performance evaluation for all queries 
>
> Instrumentation is a task that not only depends on the tool you are using, but also on the design of your code, as it ultimately determines how easy the implementation will be. In this particular case, there is no magic involved - we simply wrapped the opensearch/elk client, and tracked metrics such as feedback_search_query_total and feedback_search_query_duration_seconds.
> 
> The feedback_search_query_total metric tracks the total number of queries made to Elastic Search, counting them by consumer type and query type. On the other hand, the feedback_search_query_duration_seconds metric is a histogram used to measure the duration in seconds of interactions between the code and Elastic Search, grouped by consumer type and query type.
> 
> Choosing the right labels is crucial, as they facilitate filtering in dashboards and enable more effective monitoring of the system.


> 👘 Getting ID token by impersonating a svc-account for local environment
>
>Using svc-accounts as JSON files can be problematic, as it can be difficult to keep track of how many accounts are being used and where they are located. This approach can also be unsafe and may lead to issues such as multiple accounts being used for the same Docker image, with overlapping roles and responsibilities. To address these problems, we spent time working on a solution that synchronizes user accounts to a svc-account with the same roles and permissions as the service deployed in the cloud (staging). This means that developers can simply authenticate and start working without having to share JSON files, while the infrastructure team can maintain better control over who has access to what. This approach helps to prevent incidents and ensures that every user has appropriate access to the necessary resources, specially when developers are trying to access APIS from others teams.
>s

>🛡️Cloud Armor🛡️
>
>From what I understood, if we use the default backend security policy type (Backend security policy. LINK: https://cloud.google.com/armor/docs/security-policy-overview#policy-types), we would allow access to all IPs from all over the world to our load balancer. Another parameter would be setting the rule evaluation order to the maximum (2147483647 - why did they define such a large number???). This value ranges from 0 to 2147483647, and any rule that is smaller than 2147483647 would be applied first until access is fully validated.
>
>I also think it's worth keeping this layer 7 enabled.
>
>>The seventh layer of the OSI model, often referred to as the application layer, allows for more advanced traffic-filtering rules.
>>Rather than filtering traffic based on IP addresses, layer 7 firewalls can investigate the contents of data packets to determine whether they include malware or other cyber dangers.
>>
>Another parameter - I think we want this policy to be of the "allow" type, which means we allow anything unless we create a rule saying not to allow it.
>
>The other policy, "deny," is the opposite, meaning it doesn't allow anything until we explicitly say it can reach our backend.
>

## Q1 2023 Resume
In this quarter we brought a new senior member to the infrastructure team and we focus on improve the backlog  that we don't made in the past and besides that, improve all infrastucture in general, like, instrumenting our applications, automanting all things related to k8s/cloud/infra all of that using terraform. But I think I can sum it up, saying that the better that we do, was spread patters betweens all aplications/teams of the company. While every was focused in develop new features, we are foucused on better the Birdie system.

## Leasons
- Work with people who inspire you.
- Being a senior doesn't mean doing the same thing for many years; it means being prepared to solve and help with technical problems.
- Instrumentation, metrics, logging, and alerts are essential components of any serious system. Experienced and serious engineers know how to implement these effectively.  
- Even though I have chosen to stay in this role this quarter, I do not enjoy it. I want to be a complete software designer(because of that I put myself in this position), but focus on solve problems that teams don't wanna give a fuck, it's not a thing that I like
- Python it's not a good option for serious distributed systems, enterprises, all...
- Async is the pure gold of software development. I hope I never get into a Daily again 
    > "...the mere consciousness of an engagement will sometimes worry a whole day." – Charles Dickens


# Quarter 2 2023

* 😍 Golang cmd adpter for the extractor service
* 🐛 [Bug] Node without right scopes that impossibilitate things like, pull image
* 💡 [Tips] If you are responsible for manage a secret
* 🌱 Export elastic metrics into GCP
* 💀 [Non-tech] Product meetings is cool or not
* 🤐 Secret management solution 🤐
* 😶‍🌫️ [Tips] Push yourself to solve the problems that will agregate more value(In startup things work like that, do your best to help). Like:
    * Clients claim that queries are slow
* 💝 Remove query param from metric labels 💝
* 🐝 Difference between tracing and metrics 🐝
* 🗃 🎟 [Tips] No feedback is tougher than yours 🗃 🎟
* 💨 🎌 Gitlab -> Github Migration 💨 🎌
* 🗨 🐬 CI Learns 🗨 🐬
* 🚋 🎨GitHub Authenticating via Workload Identity Federation🚋 🎨
* 😇 Golang CRUD API 😇

---

---

## TODO
* 🤐 Secret management solution 🤐
* Metrics solution
* 🐝 Difference between tracing and metrics 🐝


---


> 😇 Golang CRUD API 😇
> 
> A simple API that interact with a database. 
> 
> ```The true learning is found in simplicity. - Jojo Mage```
> 
> Core Concepts: 
>   * API Development
>   * Database
>   * Database Migration
>   * Structured Logs
>   * Dev Container environment
>   * Go Unit test
>   * Metrics
>   * CI/CD


---

> 🗨 🐬 CI Learns 🗨 🐬
> 
> About CI, I developed some principles, like, using docker to guarantee an isolated environment, and **all steps** must be easily reproduced in your machine!
> * Try to reduce the YAML complexity, migrate your logic (keeping it simple) to Makefile
> * You just need a single branch, you can separate your environments using image artifacts and just change environment variables that are related to each one. Promote those images for each env. It's good, safe, simple, and fast.
> * Keep database/language/lint version and other parts of your code centralized, remove hard logic from Dockerfile. If you can feel that your processes are clean and easy to understand, it's a good sign that you are doing things right.
> 
--- 

> 🚋 🎨 GitHub Authenticating via Workload Identity Federation 🚋 🎨
> 
> Maintain each piece of code that you produce as IAC, is the better effort that a consistent engineer can do!  
> This workflow it's a prove of that, a lot of CLI commands, but I have certantly that after one month, problably I forgot all.
> 
> But giving that aside, we are pushing to leave svc accounts keys and use the maximum of workload identity as possible. Keep the configurations simple between external and cloud provider. In the **Gitlab -> Github Migration** we choose this approach. 
> Important resources:
> - https://gist.github.com/palewire/12c4b2b974ef735d22da7493cf7f4d37
> - https://github.com/google-github-actions/auth#setting-up-workload-identity-federation

>💨 🎌 Gitlab -> Github Migration 💨 🎌
>
>could be an excellent opportunity to push everyone to change the CI/CD details that we are talking a long time(as you said dont need to be at once...)
>
>But already thinking in that, it's not a good practice develop things that are coupled with a plataform like Gitlab, in essence it's just a Docker runner with some secrets. Details about maintain a documentation of how things are doing it's important for situations like that kkk.
>
>And looking over it, it will be at least for me a good opportunity to start thinking about decoupling what I'm developing from where it runs (already thinking about it there are some details that I don't even remember and that will make the migration process difficult , not a new tool per se, they should have it simple if they sell that solution)
>
---
> 🗃 🎟 [Tips] No feedback is tougher than yours 🗃 🎟
>
> Companies are companies, and regardless of culture, people, or good salaries, they always aim for one thing: money.
> 
> One thing that has always bothered me is that in one-on-one meetings, it is hard to give and receive real feedback (the kind that will push you back into your chair and make you think about your position), even when you are performing well. Remember, you are the best one to say whether your work is being well paid and utilized. You need to have a good critical sense, therefore it is not a good practice to stay in a comfort zone. Move yourself to always have a plan B, and don't doubt your instincts. Be a good engineer.
> 
---

>💝 Remove query param from metric labels 💝
>
>Just a tip for metrics implementation in APIs that use query parameters, if you don't remove it in the code, the cardinality of your metrics labels will increase infinitlly

---

>[Non-tech] Is Product meetings cool or not?
>
>I have concerns about this topic, for a simple reason, maybe a product driven by engineering needs to focus on engineering and anything that deviate from this topic need to be rationally thinked. The most of meetings bothered myself and I think that could be an e-mail or a async thread, and this ain't any relation with be away from people, it's just my time and time of company. The meetings that I'm like it's a meeting in Birdie that we call of **product meeting**, everyone, from each team, sumarize a topic that they work in the last 15 days and present to company. That is it. Simple and efficient.
>
---

>😶‍🌫️ [Tips] Push yourself to solve the problems that will add more value(In a startup things work like that, do your best to help). Like: Clients claim that queries are slow 😶‍🌫️
>
>I spoke with the CTO and my teammate, that I feel who my skills aren't being really used, like, learning about software foundation it's a thing that I already said is good, but it's not my focus, and for this reason, I started to think, how these skills that I'm learning and feel that is so important could be used to help Birdie achieve your PMF (Product Market Fit). **Then CTO says that clients claim that queries are slow**.
>
>* What Queries? We don't know
>* For what services. We don't know



---

> 🤐 Secret management solution 🤐
>
>How a provider like GCP doesn't have a lightwheitg solution for manage and deploy of new secrets? Really that the only solution it's either use an SKD that will give to you the certantly that you die using GCP or  save the secret into pod volume, and you need to change our code in both approaches?
>
>OK, give the rage time aside, to this task, we list some approches that all company will use to manage secrets instead of mocking them into Docker Artifacts through Gitlab variables.

---
> 😍 The Golang cmd adapter is designed to simplify the debugging of the Extractor service. The Extractor service is responsible for extracting information from a set of feedback, such as the type, sentiment, and intention for each segment of text.
> 
>     🫣 The adapter is intended to read a CSV file and generate a summary of the service's performance.

---

>🐛 [Bug] Node without the necessary scopes, making it impossible to perform certain operations such as pulling an image.
>
>The solution is to recreate all nodes, and for that, we will use a blue/green deployment approach. We will copy and paste the current node pool with the same configurations, then cordon off the old node pool. Afterwards, we will drain all old nodes. To minimize downtime and prevent issues, it is important that all deployments have the PodDisruptionBudget available.

---
>🧑🏼‍✈️  If you are responsible for manage a secret
>
> If you or our team are responsible for manage secrets/keys. Associate them to the cloud environment/login it's a good ideia. Google for example have an API to do that, then a command like: 
> ```
> gcloud secrets GROUP | COMMAND [GCLOUD_WIDE_FLAG …]
> 
> ```
> can be better than pass across a message
>
> Ref: https://cloud.google.com/sdk/gcloud/reference/secrets

---
>🤠 Export elastic metrics into GCP👹
> Ref: [ELK MANAGED](https://cloud.elastic.co/home)
>
>Using a managed database has a lot of benefits for a company (In special a Startup). Choosing the right company to do that, is also a task that needs to be done with attention. elastic.co, it's a good option, but the solution that involves metrics/loggings it's so fucking bad. 
>For this reason, exporting metrics and logging for this service to our own cloud environment and centralizing all related to monitoring, was a architectural decision made by the Infra team.
>
>Attention Points:
>
>   * Be responsable for secrets
>   * A specif ELK user to do this action (With less role permission)
>   * A sepated ELK instance to ship the logs.
>   * Metrics was exported to our GCP environment using the (ELK-prometheus-exporter)[https://github.com/prometheus-community/elasticsearch_exporter], The setup was easy, just deploy the application into our k8s cluster, and say host/user/password to access ELK from Staging and Produciton