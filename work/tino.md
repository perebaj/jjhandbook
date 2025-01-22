# What is Tino?

An attempt to create a B2B network payment system that connects suppliers and clients.

# Vocabulary

- /creditor: Someone who has the right to receive a payment from a debtor(someone else).
- Devedor/Debtor: Someone who has the obligation to pay a creditor.
- UR(Unidade de recebíveis): Imagine que você possui algum asset que ainda não está em suas mão, e você sabe que vai recebe-lô. Isso é um UR. **A nossa legislação permite que você transacione esse UR com base em algumas regras.**
Dado essas regras, é que é possível criar um mercado sobre esses URs, onde ambos os lados saem ganhando.
- **Registradora:** Uma **empresa que é responsável por registrar e manter informações sobre URs.** Disponibiliza um banco de dados digital que é acessível por todos os participantes do mercado e possibilita a realização de transações.
- Sobre as registradoras, existe uma abstração chamada **InterOP**, que é um protocolo que permite que **diferentes registradoras se comuniquem entre si.**
- **Averbação:** Um advérbio na lingua portuguese, é uma palavra que modifica o significado de um verbo. Portanto, no contexto do múndo de crédito, uma averbação é um adendo, uma complementação a um documento, fazendo com que essa nova informação seja registrada e muitas vezes pública, sendo possível realizar operaçõe com base nessa informação.
- **Credenciador/Subcredenciador**: Uma empresa que é responsável por
- **Gravame**: Uma restrição colocada sobre um bem, por exemplo, ao financiar um carro, deixar claro que esse objetivo é do banco até que o financiamento seja quitado. Ou seja, você pode usar ele por conta de um contrato.
- **Agenda de recebiveis**: lista de recebiveis de um estabelecimento.
- **Anuência**: Consentimento, permissão, aprovação.

        Um , que possui recebiveis em aberto, pode autorizar alguém a ter acesso a esses dados, esse processo de é chamado de **opt-in**, onde algumas informacoes sao compartilhadas e o acesso é concedido.
        Existe também o processo inverto de desvinculação: **opt-out**.
- KYC: Know Your Customer. It's a process that gather some information about the customer to enable or not the access to some services.
- TPV: Total Payment Volume. It's the total amount of money that is transacted in a given period.
- LTV: Lifetime Value. It's the total amount of money that a customer will spend in a given period.
- Instituicao de pagamento/FIDIC
- Culture aspects
    - The customer is the sun
    - We act in the light speed
    - Evolving day after day

Levando esses conceitos para a realidade da Tino, basicamente os assets(URs), são produtos físicos que os clientes compram,
e que ainda não estão em suas mãos.

E forma de pagamento, é o dinheiro normal, e muitas vezes, esses clientes também não possuem imediatamente o dinheiro
para pagar esses produtos, nosso trabalho, é utilizar desse mecanismo que engloba, URs, Averbações, e registradoras,
para criar um mercado de crédito, onde esses clientes possam pagar esses assets, e nós possamos receber uma porcentagem
por interligar os fornecedores com os clientes e além disso fornecer confiáveis formas de pagamentos.

Portanto criámos uma proposta de valor nas duas etapas da cadeia produtiva, befeciando os fornecedores e os clientes.

# How does it make money?

- Porcentagem sobre as transações
- Cobrança de multas e juros em caso de atraso no pagamento

# How the market works without Tino

- The supplier sells the products to customers and sometimes doesn't receive the payment immediately
the clients don't have any idea that this supplier exists
- The clients don't finalize the payment because they don't have the money

# How the market works with Tino

We create a platform that connects B2B clients and we turn viable the payment and the product delivery for both sides.

# Quarter 2 Apr-May

## Documentation

This could be a huge topic. But what I will try to tease here, is that documentation should also be **simple and easy to understand.**

The documentation space, should not be a place to navigate through a lot of information, but a place to find the key aspects of a service or the motivations behind a decision.

Here are some core topics that should be covered in the documentation:

    Objective - What is the purpose of this documentation

    How does this service/project achieve the objective?

I think that these two questions are more than enough to start a documentation.

Another important aspect. Why not place it on the GitHub?

## ASDF

- ASDF is a tool that allows you to manage multiple runtime versions with a single CLI tool.

This tool seems to cure a lot of side problems related to software development using different language versions. You can set up a box, this box uses the desired version, and inside of this box, you install your dependencies and run your code.

For Golang, I think that ASDF it's enough to manage versions, but for multiple Python projects, I am not sure if for each project the language creates an isolated environment, where each one has its dependencies.

## Makefile

Probably the starting point of a Developer in a new repository. **This file must be builded to provide a smooth and easy flow.** Otherwise, you can be expected that the left of the project will be a mess.

My golden rules:

    Create a `make help` to print all the Public targets, so the user can see what is available and just choose what is better for him/her.

    dev/
        This command should be used as a prefix to all the commands that set up some container or environment tool to run the project.

    Env: If some variable is needed to start to play, put it on the docs, if there exists some `.example` file, make it easy for the developer to copy and paste.

    tests & integration tests: Must be a rule, to run those tests in a different environment, using docker and if you want, just say what specific test case you want to run.

## Layers and frameworks

Layers over layers. This is the reality of software development that uses these fancy patterns, like hexagonal, clean, and others...
But sometimes it provides more complexity than the solution itself, it's hard to understand and maintain all the layers.

## Every company has your Moisés

Success companies had market waves that benefited them or used some external factor to grow.

## Approaching problems - my way

- walk it, talk it - Migos
- nothing better than an image to explain a problem (CREATE DIAGRAMS!)
- Take notes, share and repeat
- Build in public and every day share something

## Maybe splitting your code according to the environment is a bad idea

Those are some design decisions that I usually disagree with, like:

- Create if ENV == 'production' | 'development' | 'staging' | 'test' | 'local', and use them inside the code to create different flows, is a type of design that turns IMPOSSIBLE to test the code in different environments. For me, the code should be written in a way that you can test it in any environment.

## Pubsub gotchas

If you are using a streamlined pipeline try to save all messages that you are processing in a data warehouse using the built-in cloud solution, like BigQuery.

## I can't believe in a thing that you aren't measuring

This was a tip that I got at the GopherCon 2024, and I think that can be used in multiple contexts. Why I should trust in something that has not been measured yet? It`s the same as hiring someone without knowing his/her skills.

Measure is the key to success in convincing someone about your idea and principally to convince yourself.

## Initiatives

- Improve the async communication between the team members
    - stimulation of threads instead of calls
    - Create documents about the task and ideas before the meeting
- Improve dashboards and alerts to give more flexibility to the team
- Investigation of infrastructure problems related to cloud-run

## Hipothesis, experiments and results. A scientist building a product

- [Feature leading](https://medium.com/safetycultureengineering/an-introduction-to-feature-leading-b57906aa6767)
- Impact, Metrics and Alignment
- How capture a business goal and transform it into initiatives that we can measure and track?

## Definition of Done and definition of ready

Such a good initiative to improve the workflow of a team, especially when you are facing a problem related to time to deliver a task, maybe it can be related to the lack of information or a good step-by-step guide to follow. This initiative aims to solve this problem, by creating a checklist that must be followed, that way, the team has more visibility about what they need to implement and the possible blockers that they can face.

## Embedding external tools into internal libraries

I think that when you catch an external service and say, ok this is hard to learn, let's create an abstraction on top of it to reduce the amount of time that new guys will spend to learn it, it's a terrible idea 😅.

Doing that you create silos of knowledge and your coworkers will not be able to understand the whys and how to use these external tools which is a bad thing. You will be always forced to use a library that you don't know how it works. I'm saying that because I'm facing this problem right now.

When you need to use a new feature and extend the existing tools it's a nightmare because now, you need to learn both, the company tools and the external tools, and usually, the company tools are not well documented 🥹, so, lose-lose situation.

## My first time using business metrics to drive decisions

- Churn/Average Spending/Total Payment Volume/How happy are the customers that use a specific feature?

First experience using business metrics to drive decisions, and I think that it's a really good approach, but just works if you have a good amount of clients where you can extract some insights from them, otherwise, you will be just guessing.

## Reduce the dead letter queue messages to zero or near zero 💰

In one of my first weeks of work, I perceived a new approach that the team was using: They call it 'on-call guy', basically a guy that will spend the whole week just solving problems that show up in the production environment, these include:

- Clean/understand and fix the dead letter queue messages
- Fix the alerts that pop up in the dashboard
- Fix the problems that the clients are facing

But I perceive that most of the problems are related to JUST REPROCESS THE MESSAGES, in other words, they are paying a high salary to a guy who spends the whole week just pressing a button, freaking expensive...

My initiative was to understand what was going on, basically a wrong configuration in our Infra that limits the concurrency of messages that we can process, including cloud run tunning and pubsub retry logic with exponential backoff to

the solution was to create a workbook with the steps to fix it. Since we have more than 136 microservices to analyze, we adopt an incremental approach, and
in 2 weeks we reduce the amount of messages in the dead letter queue to zero.

Besides that I help in the creation of better DLQ visualizations, that list the time that the messages stay on the DLQs & the number of messages that should be reprocessed, this was able to enhance the way that we access and filter topics and subscriptions that were causing problems.

Moreover, this initiative reduces the time spend by team looking DLQs and we can foucus on prioritize bugs and other problems that were most relevant for the company.

## Block and Cure

Basically, the process to unblock the limit of the client's credit and readjust it to the correct value.

- Before this initiative, we had a batch process that ran some times a day to unlock the credit, but it was not efficient, since the client needed to wait a lot of time to have the credit unlocked.

- The old process also queries a lot of data from a Datawarehouse that doesn't have the tables dimensioned to support efficient queries, which means that we spend a lot of money on this process. We started to consume the data coming from a transactional database (Postgres), that was already implemented but the team didn't migrate yet.

- The new process is event-driven, which means that each time a client pays a debt, we trigger an event that will be used by the new process to unlock the credit, this process was a challenge because I had to understand all the rules that were mapped into a BigQuery View(more than 600 lines) and transform it into a code that can be easily understood, tested, and maintained.
    - I was recognized by the team for this initiative because the whole process was documented, so a black box was transformed into a white box.
    - Basically, we reduce the time to unlock the credit from 1 day to minutes, so the client can buy more, and we contribute to the company Goal. Increase the average spending of the clients.
    - I follow all the rules of the company, including using the existing libraries to structure logs, metrics and alerts to monitor the process.
- Reduce the amount of tickets to the support team to near zero. Happy clients.

# Quarter 3 Jun-Aug

## Q3 Brieding

## Q3 Lessons

## Migrate old provider endpoints to the new one

Simple task to understand what changes are needed to migrate a third-party provider to a new one. The main goal was to understand the differences between the APIs and payloads and finally map what were the changes in the code base to support the new provider.

In this process, I mapped all routes in our code base that were using the old provider and all functions, created a miro board where the team could see a big picture of what was going on, and this was useful for splitting the tasks between the routes that have less overlap.

This process includes some calls with the new provider to understand the new API and how to use it.

## Reduce manual labor using Retool and application data

## Fraud flow for first purchases

# Quarter 4 Sep - Dec

## Q4 Briefing

## Q4 Lessons

## Drammatically reduce the credit concession

Experience in the credit flow

## Rollout new fraud engine for first purchases

brov, this task takes almost 1 quarter. WTF

## Implement a fallback mechanism to speed up the credit concession

### What is a fallback mechanism and why it's important?

Imagine that we are accessing multiple providers to request data about a client or even to request internal data. But those providers somethimes have problems and instabilities(maily related to time). And this flow that we are creating is time-sentive. The solution:

## Reduce the credit concession to 40 seconds in p90.

- Begging of the quarter just 45% of our clients were served in the right time. After some changes we where able to serve 90% of the clients in the right time.
- The remaining 10% were related to deploys that affect the time our problems related to the provider that we are using to request data.
- To circumvent it we create a fallback mechanism that controls the time that each providers will wait to respond and we adapt the ML models to retrive the same results when we don't have the data for all the providers.

## Why this should be rethinked?

- The current flow doesn't have a simples way to be tested, deployed and maintained
    - The only way that the engineer team found to achieve it, was creating a hard dependency between BAs and the engineering team, we build some visualizations on top of the data that were saved in our warehouse.
    - Deploys that affect the concession flow. When we decide to deploy a new version of the service, our service crashes, breaking the concession flow.
    - High costs related to infraestructure. The widespread cloud run usage is a think that we need to rethink. We duplica code, duplicate infra, increasing the tail scale network problems.

## Reduce fraud for a new provider creating a side flow (Samsung)
    - Serve 100% of the requests in less than 5 seconds
        - Tunning in the infraestructure to support more requests and tunning python workflows to avoid lock in the event loop and using threads to load ml models in a single shot.

## Communication between subscriptions and multiple cloud run services

When we are dealing with mulitple push subscriptions and multiple cloud run services, we need to be aware of some important aspects:

- oidc token
- audience

## Fix Fraud flow for creating a decision tree flow

Adaptação rápida com virada p/ produção com observabilidade e testes, atendendo uma necessidade urgente do time de negócios que estava enfrentando problemas críticos com criminosos que encontraram uma brecha no sistema.

## Follow the DLQ is not enogh

Criamos uma nova subscription para consumir uns dados em um fluxo paralelo, e isso fez com que o tempo de crédito fosse afetado, mesmo dividindo a um nível físico. Nossos alertas não pegaram esse problema vimos através de dashboards que estavamos com problema.

- A ideia aqui, foi melhorar os dashboards e criar alertas sobre esse problema detectado.
- Encontramos os clientes que ficaram travados
- Notificamos a empresa sobre o problema
- Efetuamos o rollback do deploy
- Reprocessamos os clientes

# Why have specialist in the current software solutions is so important?

Recently, we are working in a team that was not responsible to create and maintain the software that we are using, so, we are facing small issues of someone that don't know how the software works, details that are very important to enhance the software and attend the business needs.
This give to us a huge opportunity to learn and propose changes that can improve the software and the business. But at same time, we are broking the flow in a constant pace.


# The bug that crashes the payload response and return a timeout for your clients, so fraudulent transactions are made

- We are sending a response with a broken JSON, which is causing a problem when we are trying to unmarshal the response, and this is causing a timeout in the client side, so the client is able to make a fraudulent transaction. The issue behind it, was related to a internal library that use a middleware to handle the response, and this middleware was injecting broken data in the response. We fix it by removing the points in the code that use this middleware.

# Quarter 1 2025

- Frustration: Lead a project where I decided to adopt a light structure, that were agains the default company one(I thought that this one was too heavy). For this reason I generate some circular dependencies between my mocks and interfaces that were blocking my team to work smootly and ship their code.

The principal lesson learned was that I spend my weekend studing about this topic, craete an essay about it and learn something about rush.

- Sometimes, you can build a scrapy design, run it manually and harvast the same outcomes. It will delivery fast and you don't need to what the whole process to ship something to production to see things working. Reduce steps = avoid mistakes when they can not exists
