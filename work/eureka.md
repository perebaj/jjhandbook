# Eureka

A nice side project, Eureka is facing some problems related to unstructured data and trying to find some insights into it. My job is to unravel this data and push the right answers based on this data

# Quarter 4 - 2023

## SQL and NoSQL

Each type of database has its properties, and having these concepts well defined before design and proposing structural changes or even creating new features are fundamental to increment projects. Eureka was a problem that I called: 

`We don't have sure how to deal with our data.`

They need to analyze big-time windows of data and based on that, single out what is more important to drive new decisions.

What they proposed, was to send data from different databases to an ElasticSearch cluster using Airflow to manage it. But the problem itself doesn't fit with that. The principal goal of Elastic isn't to gather different structures and answer business questions, isn't possible to use "JOIN" statement and it doesn't even have simple SQL commands, Elastic is a NoSQL Db 😷! So, wrong db to the problem that they have

So, what was my proposal here?

Let's create a dataflow into GCP just using GCP pipelines and Bigquery, and to visualize this data, let's use Looker Studio, simple and cheap, at least for the size of their tables and the query volume

