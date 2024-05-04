# Saga

Imagine that you have a system, and you decide to split it into multiple services(software components) that will be responsible for calculating and storing some data.

When you think about your pattern decision, each service has its own DB, and the problem arises when you need to make a transaction that involves multiple them. The question is:

- How can you guarantee this transaction?
- How can you guarantee that if something goes wrong you will have the ability to rollback the changes?
