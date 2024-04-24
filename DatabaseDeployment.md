# Introduction
If you are a developer and in the tech world, you must have heard about databases. This is the heart of information that organizes your data well-mannered. Its properties like atomicity, consistency, isolation, and durability (ACID) ensure that your transaction is processed accurately.
There are so many database services available in today's world that we can choose any of them or use multiple of them together based on our use cases. 

The challenge is always having our database schema (structural or data changes) that needs to be promoted or synced to different working environments regularly because our needs change over time. This article will show you one of the popular methods to promote your database changes to different environments.

# What do we want to do?
I've made some changes in my database schema in the local environment and now this needs to deploy to other environments like we do with application code and this should be automated.

# How?
In this article, we are using the following tools to demonstrate database publishing:
1. SQL Server 2022
2. Azure DevOps Express
3. Azure Data Box
   
## Begin the deployment now
1. You must know the correct credential to connect with your database.
2. 
