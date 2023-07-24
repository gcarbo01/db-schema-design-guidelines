# db-schema-design-guidelines
Database SQL schema design and data modelling guidelines

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./images/db-schema-design-guidelines.jpg">
  <source media="(prefers-color-scheme: light)" srcset="./images/db-schema-design-guidelines.jpg">
  <img alt="Shows an illustrated sun in light mode and a moon with stars in dark mode." src="./images/db-schema-design-guidelines.jpg">
</picture>


## Intro
### Summary
This document lists different patterns for logical and physical data modelling. <br>
The list is not the full list of guidelines but a collection of best practices to be considered when producing logical, physical data models and avoiding re-work or technical debt. 
### Architecture Governance
Architects can benefit from database SQL schema and data modelling guidance from several viewpoints. <br>
**Standardization of data modelling** <br> 
Guidelines and standard SQL schema designs ensure consistency and standardization across projects, fostering a unified data architecture and facilitating collaboration. <br>
**Improved Data Quality** <br>
 Following guidelines leads to data normalization, validation, and integrity, improving data quality, reducing inconsistencies, and enhancing data reliability. <br>
**Scalability** <br> 
Best practices in data modelling cater to scalability, allowing data structures to accommodate growth and business changes without major restructuring or performance issues. <br>
**Efficient Data Retrieval** <br>
Logical and physical data modelling patterns optimize data storage and retrieval, leveraging techniques like indexing and columnar storage to boost system performance.  <br>
**Easier Maintenance** <br>
Well-designed data models ease database schema maintenance, minimizing schema changes and avoiding technical debt.  <br>
**Enhanced Security** <br>
Data modelling guidelines incorporate security measures, protecting sensitive data, compliance with privacy regulations, and safeguarding organizational data.  <br>
**Alignment with Business Goals** <br>
Architects align the data model with business goals and requirements, which is crucial for delivering valuable insights and supporting decision-making processes.  
## Capabilities
Listed below are capabilities in no particular order. Do your research. Most of these capabilities provide some references to tools and libraries. Most of them are open-sourced. These may flourish or die without notice. <br>
Each of the capabilities may apply to one or more Categories. So they are tagged by "Category".
<br>
<br>
<br>
# SQL Styling
## Category
## Comments
It is strongly recommended to read, understand and incorporate SQL Styling standards. <br>
## Resources
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
## Comments
It helps you write good SQL and catch errors and bad SQL before it hits your database. <br>
In addition, if the project contains “.sql” files, it is recommended to incorporate a SQL Linter into the CI-CD pipeline.  <br>
For example:
## Resources
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
## Comments
There are several theories about the design of primary keys. The discussion about the primary key can be of various kinds. <br>
Firstly, avoid multi-column primary keys. These can introduce complexity and make it difficult to query.  <br>
Secondly, the design of the primary key high-performance systems is different from enterprise platforms.  <br>
So, in most cases, for enterprise applications, we advise using a surrogate primary key of the type GUID. Although these are not human-friendly, in most scenarios are better than other options, such as Natural Keys, Business keys, and also auto-generated sequential integers.  <br>
Also, avoid other bad options such as simple Date-time or composed keys, for example: <br>
``FirstName-LastName-City-TimeStamp``, or <br>
``Product-ProductPart-ProductCode``, etc. <br>
If it is required to have displayable user-friendly ids for the end-user, it is better to adopt other design strategies. See the Custom Human Readable id and Mnemotechnical hash id sections of this document.  <br>
This category of IDs must also be considered if the system will be implementing APIs. The Restful API URLs are assumed to be used by humans. <br>
## References:
https://vertabelo.com/blog/primary-key/ 
<br>
<br>
<br>
# Unique Id – GUID or UUID or GUID
## Category
## Comments
**Introduction** <br>
It is the design for a well-constructed Primary Key. It is called UUID or GUID type. <br>
It is also known as “UniqueIdentifier”. This key also fits into the category of the surrogate artificial Key.
This is an Internet Engineering Task Force (IETF), an international standard managed by a specification that, at the time of writing this document, was in version 4.122.
The purpose of this key is to have an identification that brings some indirection to relationships among entities and makes refactoring, migrations, re-building, and re-indexing of microservices databases easier.  <br>
This field has to be configured as "Unique" in the table definition.  <br>
Using this key to all tables on microservices allows refactoring and migrating data without having to worry about the physical and business keys, which you do not control.<br>
Since our system is not based on a single relational database, we must have a strategy for these "soft foreign keys" across microservices. <br>
So microservices can generate this key and provide it to other microservices for reference. <br>
If we need to re-build the DB, the database engine may assign a different one if we were using the physical key auto-generated sequential. But with this unique key will be the same, and your relationships across multiple DBs, the integrity of the overall system will remain intact. <br>
Business keys could be better, too, for other reasons. Law changes and regulations can badly affect systems based on an external business key we do not have any control over.  <br>
As this type of id is to identify a row in the database, this pattern also applies to foreign keys.  
**Format** <br>
The format of this key is of the format: ``NNNNN-NNNNN-NNNNN-NNNNN``  <br>
Where ``‘N’`` is Alphanumeric:``[0-9] | [A-Z] | [a-z]`` <br>
Example: ``A2eXh-HBwHj-Gd04t-zezmP-ojU65`` <br>
For more information, see surrogate Key pattern references.
**Schema Field definition** <br>
The convention for this field can be: <br>
``Varchar (50) - ``primary Key``. <br>
**Naming Convention** <br>
The name of the field should be: <br>
``<The same name used on the table>`` + ``“Id”`` <br>
For example:<br>
<code style="color : blue">Contract</code> (table name) + <code style="color : blue">Id</code> = <code style="color : blue">``ContractId``</code> <br>
**Unique Identifier – Suffix**  <br>
The rationale for this convention is for clarity when querying multiple tables.  <br>
For example, if the unique identifiers of each table are only ``Id``, a select query will bring all columns names of both tables, but they would have indistinguishable names; both will be ``Id``.  <br>
So, by adopting this convention, each of these columns will have its unique name. <br>
**Time-creation awareness** <br>
This was introduced in the MongoDB implementation.<br>
This implementation caters to the Id to be sortable by time-creation using: <br> 
``ObjectId.getTimestamp()``, which returns the timestamp portion of the object as a Date.<br>
This is optimal for database sharding.<br>
For more information, see MongoDB Object id implementation references.<br>
**Centralised service - IDs generation** <br>
This can be done by a dedicated API that creates an Id. <br>
For example, Twitter’s Snowflake implements a  Thrift service that uses Apache ZooKeeper to coordinate nodes and then generates 64-bit unique IDs. 
In the case of needing a centralised service to generate Ids, there may be a need to be highly performant and available. Or generate Ids in batches and use a buffer using a Cache platform. <br>
## References
**UUID V4 Random Generation standard** <br>
UUID V4 Random Generation. Version 4 generates a Unique ID based on random number generation. … Version 4 is also commonly referred to as a GUID. While a GUID doesn’t follow the same specification as UUIDs, it is the same basic format.<br>
https://datatracker.ietf.org/doc/html/rfc4122  <br> 
**Wikipedia GUID** <br>
https://en.wikipedia.org/wiki/Universally_unique_identifier <br>
**JS UUID library** <br>
A library to generate the unique IDs <br>
https://www.npmjs.com/package/uuid <br>
**Surrogate key** <br>
Surrogate Key pattern <br>
https://en.wikipedia.org/wiki/Surrogate_key
**UUId - MongoDB implementation** <br>
This was introduced in the MongoDB implementation <br>
https://www.mongodb.com/docs/manual/reference/method/ObjectId/  <br>
<br>
<br>
<br>
--End of File--
