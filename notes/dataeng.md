# Data Engineering

IMO, Data Engineering is just a sub-topic behind software development, but with a focus on data. When I started to perceive that my skills were too focused on this field, I started to stay worried. This isn't a way that I want to track my career, but is the way that the industry wants!

Creating commodities upon developers' experience it's a good approach for the industry, with that, you can reduce the salaries and hire more people and fast. For me, this isn't the way, my way is being close to the wide range of software development approaches and having contact with all things that I can.

But yep, I need to remember some data engineering concepts, and here is the place that I will put them.

## Parquet

It is a columnar storage format, available for any project in the Hadoop ecosystem. It was designed to efficiently store and process large amounts of data. But, what the fuck is that?

Imagine that you have the following table

| Transaction ID | Date       | Customer ID | Product ID | Quantity | Unit Price |
|----------------|------------|-------------|------------|----------|------------|
| 1              | 2023-01-01 | 101         | 001        | 2        | 10.99      |
| 2              | 2023-01-02 | 102         | 002        | 1        | 25.50      |
| 3              | 2023-01-03 | 103         | 003        | 3        | 15.75      |
| 4              | 2023-01-04 | 104         | 001        | 2        | 10.99      |
| 5              | 2023-01-05 | 105         | 002        | 1        | 25.50      |

Now let's imagine that you choose the CustomerID column to use this **"column storage format thing"**, by doing that, you are just increasing the cardinality of your table, because it's obvious that you have one ID per Customer, so not a good choice. But, doing that using the Data, specifically the **Day**, you could return a huge amount of data very fast just doing that.

```sql
SELECT * FROM table WHERE Date = '2023-01-01'
```

Doing that, with a parquet file, will interpret the WHERE clause, just access the **Folder** in our distributed Storage, and return all the data that you need.

The distributed storage will have the following structure

```
    Partitions:
        2023-01-01
        2023-01-02
        2023-01-03
        2023-01-04
        2023-01-05
```

This is a very simple example, but it's the idea behind the parquet file.

## OLAP and OLTP

Simply, OLAP is for **Data Warehousing** and OLTP is for **transactional systems.**

# Resource

- [Data Engineering Wiki](https://dataengineering.wiki/Index)
- [Big query Tutorials](https://count.co/sql-resources/bigquery-standard-sql/window-functions-explained)
