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

# Quarter 1

## Documentation

This could be a huge topic. But what I will try to tease here, is that documentation should also be **simple and easy to understand.**

The documentation space, should not be a place to navigate through a lot of information, but a place to find the key aspects of a service or the motivations behind a decision.

Here are some core topics that should be covered in the documentation:

    Objective - What is the purpose of this documentation

    How does this service/project achieve the objective?

I think that these two questions are more than enough to start a documentation.

Another important aspect. Why not place it on the GitHub?

## Meetings

<TODO>

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

## Cloud Run x Kubernetes

Sharing some experiences that I had with both services and where I think that each one fits better.

TODO

## Detect duplicated messages when using PubSub

TODO

## Every company has your Moisés

Success companies had market waves that benefited them or used some external factor to grow.

## Approaching problems - my way

- walk it, talk it - Migos
- nothing better than an image to explain a problem (CREATE DIAGRAMS!)
- Take notes, share and repeat
- Build in public and every day share something

## Due debt

It was my first solo task at this company, and I was very excited to deliver it.

The problem that we are trying to solve:

    - Tempestivity: The time that we take to unlock the client's credit after he/she pays the debt.
    - Save money: Reduce the amount of money spent on the process, today we use a not efficient BQ view that scans a lot of data to give this result.
    - Debug: As we have just a query, it's hard to debug and understand what is happening after the flow, so we can't see what was the decision and why it was taken.

The solution that I proposed:

    Create an event-driven pipeline that is triggered every time that a client pays a debt and we calculate if we need to unlock based on a set of predefined rules/criteria.

The result: <TODO>

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
