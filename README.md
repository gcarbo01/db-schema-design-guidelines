# db-schema-design-guidelines
Database SQL schema design and data modelling guidelines

<img src="./images/db-schema-design-guidelines.jpg" align="center" width=50% height=50%>

## Intro
### Summary
This document lists different patterns for logical and physical data modelling. It can read as a cookbook for good data modelling and SQL schema design. <br>
Of course, this list is always being updated, and the list never will be completed or finished. It may not answer your problem today. But I have been collecting scenarios to say that it is the most comprehensive list of guidance. So, I present this list as the best advice for several situations and scenarios; with all humility, this will be useful for most professionals and organisations trying to standardise data design. This is presented as a list of guidelines and best practices to consider when producing logical, physical data models and avoiding re-work or technical debt. The subjects have a heavy enterprise and solution architecture and design viewpoint, this is because it is most likely to be used by those trying to instrument governance over development groups.
### Architecture Governance
Architects can benefit from database SQL schema and data modelling guidance from several viewpoints. <br>
<br>
**Standardization of data modelling** <br> 
Guidelines and standard SQL schema designs ensure consistency and standardization across projects, fostering a unified data architecture and facilitating collaboration. <br>
<br>
**Improved Data Quality** <br>
 Following guidelines leads to data normalization, validation, and integrity, improving data quality, reducing inconsistencies, and enhancing data reliability. <br>
 <br>
**Scalability** <br> 
Best practices in data modelling cater to scalability, allowing data structures to accommodate growth and business changes without major restructuring or performance issues. <br>
<br>
**Efficient Data Retrieval** <br>
Logical and physical data modelling patterns optimize data storage and retrieval, leveraging techniques like indexing and columnar storage to boost system performance.  <br>
<br>
**Easier Maintenance** <br>
Well-designed data models ease database schema maintenance, minimizing schema changes and avoiding technical debt.  <br>
<br>
**Enhanced Security** <br>
Data modelling guidelines incorporate security measures, protecting sensitive data, compliance with privacy regulations, and safeguarding organizational data.  <br>
<br>
**Alignment with Business Goals** <br>
Architects align the data model with business goals and requirements, which is crucial for delivering valuable insights and supporting decision-making processes.  
## Capabilities
I've listed the capabilities in no particular order. Most of these capabilities provide some references to tools and libraries. Most of them are open-sourced. These may flourish or die without notice. 
## Use
After reading any of the capabilities, I recommend doing your own research. If you have comments or disagree, or find gaps, pls reach out, collaborators are welcome to the project.<br>
Each of the capabilities may apply to one or more Categories. So they are tagged by "Category".
<br>
<br>
<br>
# SQL Styling
## Category
## Description
It is strongly recommended to read, understand and incorporate SQL Styling standards. <br>
## References
GitLab SQL Styling best practices <br>
https://about.gitlab.com/handbook/business-technology/data-team/platform/sql-style-guide/  <br>
SQL Style Guide- (by Simon Holywell) <br>
https://www.sqlstyle.guide/ <br>
Mozilla Data Documentation - SQL Style Guide <br>
https://docs.telemetry.mozilla.org/concepts/sql_style.html 
<br>
<br>
<br>
# SQL Linter 
To be included in the CI-CD pipeline
## Category
## Description
It helps you write good SQL and catch errors and bad SQL before it hits your database. <br>
In addition, if the project contains “.sql” files, it is recommended to incorporate a SQL Linter into the CI-CD pipeline.  <br>
For example:
## References
**SQL Fluff	Source code quality	SQL linter** <br>
Source code linter specialising in SQL statements.	SQL Linter <br>
https://docs.sqlfluff.com/en/stable/ <br> 
**SQL Lint	Source code quality	SQL Linter**<br>
Source code linter specialising in SQL statements.	SQL Linter  <br>
https://github.com/joereynolds/sql-lint
<br>
<br>
<br>
# Primary Key design
## Category
## Description
There are several theories about the design of primary keys. The discussion about the primary key can be of various kinds. <br>
Firstly, avoid multi-column primary keys. These can introduce complexity and make it difficult to query.  <br>
Secondly, the design of the primary key high-performance systems is different from enterprise platforms.  <br>
So, in most cases, for enterprise applications, we advise using a surrogate primary key of the type GUID. Although these are not human-friendly, in most scenarios are better than other options, such as Natural Keys, Business keys, and also auto-generated sequential integers.  <br>
Also, avoid other bad options such as simple Date-time or composed keys, for example: <br>
``FirstName-LastName-City-TimeStamp``, or ``Product-ProductPart-ProductCode``, etc. <br>
If it is required to have displayable user-friendly ids for the end-user, it is better to adopt other design strategies. See the Custom Human Readable id and Mnemotechnical hash id sections of this document.  <br>
This category of IDs must also be considered if the system will be implementing APIs. The Restful API URLs are assumed to be used by humans. <br>
## References
https://vertabelo.com/blog/primary-key/ 
<br>
<br>
<br>
# Unique Id – GUID or UUID or GUID
## Category
## Description
### Introduction
It is the design for a well-constructed Primary Key. It is called UUID or GUID type. <br>
It is also known as “UniqueIdentifier”. This key also fits into the category of the surrogate artificial Key.
This is an Internet Engineering Task Force (IETF), an international standard managed by a specification that, at the time of writing this document, the latest version is rfc4122 v4.122.
The purpose of this key is to have an identification that brings some indirection to relationships among entities and makes refactoring, migrations, re-building, and re-indexing of microservices databases easier.  <br>
This field has to be configured as "Unique" in the table definition.  <br>
Using this key to all tables on microservices allows refactoring and migrating data without having to worry about the physical and business keys, which you do not control.<br>
Since our system is not based on a single relational database, we must have a strategy for these "soft foreign keys" across microservices. <br>
So microservices can generate this key and provide it to other microservices for reference. <br>
If we need to re-build the DB, the database engine may assign a different one if we were using the physical key auto-generated sequential. But with this unique key will be the same, and your relationships across multiple DBs, the integrity of the overall system will remain intact. <br>
Business keys could be better, too, for other reasons. Law changes and regulations can badly affect systems based on an external business key we do not have any control over.  <br>
As this type of id is to identify a row in the database, this pattern also applies to foreign keys.  
<br>
### Format
The format of this key is of the format: ``NNNNN-NNNNN-NNNNN-NNNNN``  <br>
Where ``‘N’`` is Alphanumeric:``[0-9] | [A-Z] | [a-z]`` <br>
Example: ``A2eXh-HBwHj-Gd04t-zezmP-ojU65`` <br>
For more information, see surrogate Key pattern references.<br>
 <br>
### Schema Field definition 
The convention for this field can be: <br>
``Varchar (50)`` - ``primary Key``. <br>
<br>
<img src="./images/PrimaryKeydefinition1.jpg" width=60% height=60%>
<br>
<br>
``Unique``<br>
<br>
<img src="./images/PrimaryKeydefinition2.jpg" width=60% height=60%>
<br>
<br>
### Naming Convention
The name of the field should be: <br>
``<The same name used on the table>`` + ``'Id'`` <br>
For example: ``Contract`` (table name) + ``Id`` =  ``ContractId`` <br>
<br>
<img src="./images/PrimaryKeydefinition3.jpg" width=30% height=30%>
<br>
### Unique Identifier – Suffix
The rationale for this convention is for clarity when querying multiple tables.  <br>
For example, if the unique identifiers of each table are only ``Id``, a select query will bring all columns names of both tables, but they would have indistinguishable names; both will be ``Id``.  <br>
So, by adopting this convention, each of these columns will have its unique name. <br>
<br>
### Time-creation awareness
This was introduced in the MongoDB implementation.<br>
This implementation caters to the Id to be sortable by time-creation using: ``ObjectId.getTimestamp()``, which returns the timestamp portion of the object as a Date.<br>
This is optimal for database sharding.<br>
For more information, see MongoDB Object id implementation references.<br>
<br>
### Centralised service - IDs generation
This can be done by a dedicated API that creates an Id. <br>
For example, Twitter’s Snowflake implements a  Thrift service that uses Apache ZooKeeper to coordinate nodes and then generates 64-bit unique IDs. 
In the case of needing a centralised service to generate Ids, there may be a need to be highly performant and available. Or generate Ids in batches and use a buffer using a Cache platform. <br>
## References
**UUID V4 Random Generation standard** <br>
UUID V4 Random Generation. Version 4 generates a Unique ID based on random number generation. … Version 4 is also commonly referred to as a GUID. While a GUID doesn’t follow the same specification as UUIDs, it is the same basic format.<br>
https://datatracker.ietf.org/doc/html/rfc4122  <br> 
 <br>
**Wikipedia GUID** <br>
https://en.wikipedia.org/wiki/Universally_unique_identifier <br>
 <br>
**JS UUID library** <br>
A library to generate the unique IDs <br>
https://www.npmjs.com/package/uuid <br>
 <br>
**Surrogate key** <br>
Surrogate Key pattern <br>
https://en.wikipedia.org/wiki/Surrogate_key <br>
 <br>
**UUId - MongoDB implementation** <br>
This was introduced in the MongoDB implementation <br>
https://www.mongodb.com/docs/manual/reference/method/ObjectId/  <br>
<br>
<br>
<br>
# Unique Id – Prefix
## Category
## Description
This pattern adds a determined prefix to a specific business object id. This is required in some business scenarios.
### Case 1 – Object Type
This is the case when it is required to identify the object type id by only observing the UUID. And, with the UUID alone, this is not enough.  <br>
For example: <br>
Customer ID (``cus_``) is used as the prefix of the UUID for Customer entities. <br>
So, a customer UUId will look like this:  ``cus_A2eXh-HBwHj-Gd04t-zezmP-ojU65`` <br>
Account ID (``acct_``) is used as a prefix in the UUID for Accounts entities. <br>
So, an account UUId will look like this:  ``acct_3133a6ba-4392-4223-b678-84b6c044e5bf`` <br>
### Case 2 – Object Creator
This is the case when it is required to identify the creator of the entity. In this case, multiple systems can create objects of the same type, and it is required that the creator of the object instance is identified easily by only observing the UUID. And, with the UUID alone, this is not enough. <br>
For example: <br>
An enterprise generates payment transactions from different channels: Web, Mobile, ATM, BackOffice, etc. <br>
Then each channel will use the assigned channel ``<prefix>`` when creating the payment transaction + ``UUId``. <br>
Web:           ``web_c9500552-f07d-4ac4-9124-7f317080fe00`` <br>
Mobile:        ``mob_c6c043e1-59f7-45eb-b372-9bf252552117`` <br>
ATM:           ``atm_6b2c9ab6-c1b3-469f-beda-a170465fe8d2`` <br>
Back-Office:   ``bao_7e4df0ad-9e0d-47e9-8604-293480da3301`` <br>
## References
# Unique Id – Self-generated sequential id
## Category
## Description
This is a type of property that can be assigned to fields that make them auto-incremental.  Generally, when used on tables, this field type is used intentionally as a Primary Key. 
However, the advice is not to use this practice. <br>
The reason for this is that this auto-generated number is the physical row in the table and may interfere with the refactoring and migration of data in microservices. <br>
It has been observed that the database engine can automatically create a field called “id” when creating the table and set it as Primary Key. This may happen if the script that creates the table does not have an explicit Primary Key. So, the database engine creates a default one, auto incremental.  <br>
But we expect this not to happen if a Primary Key is specified explicitly. <br>
<br>
<img src="./images/auto-incremental-id.jpg" align="center" width=60% height=60%>
 <br>
**Caution**<br>
Sequential integer IDs are considered a vulnerability. It leaves the system wide open to enumeration attacks, where it becomes trivially easy for malicious actors to guess IDs that they should not be able to since your IDs are sequential.
## Resources
 


<br>
<br>
<br>

--End of File--
