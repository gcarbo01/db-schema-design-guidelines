# db-schema-design-guidelines
Database SQL schema design and data modelling guidelines

<img src="./images/db-schema-design-guidelines.jpg" align="center" width=50% height=50%>

## Intro
### Summary
This document lists different patterns for logical and physical data modelling. It can read as a cookbook for good data modelling and SQL schema design. <br>
Of course, this list is always being updated, and the list never will be completed or finished. It may not answer your problem today. But I have been collecting scenarios to say it is the most comprehensive list of guidance I know. <br>
So, I present this list as the best advice for several situations and scenarios; with all humility, this will be useful for most professionals and organisations trying to standardise data design. This is presented as a list of guidelines and best practices to consider when producing logical, physical data models and avoiding re-work or technical debt. <br> The subjects have a heavy enterprise and solution architecture and design viewpoint because it is most likely used by those trying to instrument governance over development groups.
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
I've listed the capabilities in no particular order. <br> It is important how to read them; the capabilities may overlap, or some of them are subspecialisation of others. Some of them are complimentary. So, I organise them into topics or subjects that, in most cases, can be read independently and also implemented independently, with obvious exception of course.
Lastly, it is important to note that most of these capabilities provide some references to websites, tools and libraries. Most of them are open-sourced. These may flourish or die without notice. 
## Use
After reading any of the capabilities, I recommend doing your own research. Please let me know if you have comments, disagree, or find gaps. Collaborators are welcome to the project.<br>
Each of the capabilities may apply to one or more Categories. So they are tagged by "Category".
## Disclaimer
There are lots of original material on this page. However, I have read and collected information from other places too. In most cases, I summarised and wrapped the information with my flavour. In these cases, I kept the links to the original sources as much as possible for reference if I missed some. My apologies. It is not my intention to take credit for other people's work.
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
First, please avoid multi-column primary keys. These can introduce complexity and make it difficult to query.  <br>
Secondly, the primary key for high-performance systems' design differs from enterprise platforms.  <br>
So, in most cases, we advise using a surrogate primary key of the type GUID for enterprise applications. Although these are not human-friendly, most scenarios are better than other options, such as Natural Keys, Business keys, and auto-generated sequential integers.  <br>
Also, avoid other bad options such as simple Date-time or composed keys, for example: <br>
``FirstName-LastName-City-TimeStamp``, or ``Product-ProductPart-ProductCode``, etc. <br>
If it is required to have displayable user-friendly ids for the end-user, it is better to adopt other design strategies. Please review this document's Custom Human Readable Ids and Mnemotechnical hash id sections.  <br>
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
Using this key to all tables on microservices allows refactoring and migrating data without worrying about the physical and business keys you do not control.<br>
Since our system is not based on a single relational database, we must have a strategy for these "soft foreign keys" across microservices. <br>
So microservices can generate this key and provide it to other microservices for reference. <br>
If we need to re-build the DB, the database engine may assign a different one if we use the physical key auto-generated sequential. But with this unique key will be the same, and your relationships across multiple DBs, the integrity of the overall system will remain intact. <br>
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
<img src="./images/PrimaryKeydefinition3.jpg" width=45% height=45%>
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
You can see MongoDB Object id implementation references if you want more information.<br>
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
For Customer ID (``cus_``) is used as the prefix of the UUID for Customer entities. <br>
So, a customer UUId will look like this:  ``cus_A2eXh-HBwHj-Gd04t-zezmP-ojU65`` <br>
For Account ID (``acct_``) is used as a prefix in the UUID for Accounts entities. <br>
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
  <br> <br>  <br>   
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
## References
 <br> <br><br>
# Business Keys
## Category
## Description
Other variations for Business keys are artificial identifiers assigned to identify a business entity uniquely. <br>
They are, in most cases, they have the property of being humanly readable. <br>
Natural Keys and Business keys could be alike. <br>
For example:   <br>
. “Asset Number”  <br>
. “Invoice number” <br>
<br>
Be aware that there could be business rules that apply to this entity that may require these Natural Keys fields to be assigned the SQL properties: <br>
. ``NOT NULL``<br>
. ``UNIQUE``<br>
So, please use these accordingly.
<br> <br><br>
# Natural Keys
## Category
## Description
They are natural attributes of entities that are used in the real world.  Natural Keys and Business keys could be alike. <br>
Because they are: <br>
. Unique  <br>
. Mandatory  <br>
. Immutable  
<br>
For example: <br>  
. "ABN" (Australian Business Number) <br>
Australia Business Registry generates these identifiers, which are used for Business Organisations.  <br>
. "Employee number" <br>
   The HR system generates this <br>
 <br>
Be aware that there could be business rules that apply to this entity that may require these Natural Keys fields to be assigned the SQL properties: <br>
. ``NOT NULL``<br>
. ``UNIQUE``<br>
So, please use these accordingly.
## References
<br> <br><br>
# Identification pattern
This pattern is one of the most popular when dealing with entities that can have multiple identifiers from multiple sources. So, it is one of the most important patterns to be considered and adopted.
## Category
## Description 
Given a business entity table, the pattern creates a separate table called ``Identification``related to a business entity table. One-to-Many relationship. <br>
It is used to signify that a business entity is stored locally, but it is a copy.  <br>
The Identification table keeps other identifiers of the business entities and metadata about the identification itself. <br>
For example: <br>
. The original name of the Key. <br>
. The Value of the Key  <br>
. The origin application. <br>
About the possible scenarios where extra identifiers are needed to be placed in the Identification table:	<br>
. In the scenario where the system stores a copy of an object or business entity that is not mastered in this platform.	So the original identifier is preserved.<br>
. In the scenario where more than one identifier for an object is needed, the identifiers are placed separately in a different table. <br>
. In the scenario where extra identification of an object is needed, it must be kept in the original format and consist of a composed key with more than one field.<br>
. In the scenario where the old identifiers of objects must be preserved after migrating to a new system. 	<br>
	<br>
So, in conclusion, this pattern is useful when it is required a system keeping information about how an object is identified in another system. <br>
In the case of implementing an eco-system with multiple microservices, and they are all considered within the boundaries of the same system, this pattern is not required. Just use the Unique Identifier directly. <br>
However, the following scenarios can be useful: <br>
A system keeping information about the entities mastered in CRMs,  <br>
. MS D365 (ERP) <br>
. Zoho CRM (Presales) <br>
. etc <br>
### Logical modelling
<br> <img src="./images/Identification-pattern-logical.jpg" align="center" width=60% height=60%> <br>
### Physical design
<br><img src="./images/Identification-pattern-physical.jpg" align="center" width=100% height=100%> <br> <br>

| #	| Column  Name	| Description |
| --- | --- | --- |
|1|		IdentificationId |	Business id. Unique identifier |
|2|		originApplicationId	| Application id. Application mastering this business entity. This is a soft link, not a foreign key.|
|3|		Name |	Name of the attribute as it is known in the origin application.|
|4|		Value |	Value of the attribute. |
 
### Hypothetical example scenarios
#### Example 1
An account is an object that is mastered in Dynamics. But a copy is kept in a microservice. 
Therefore, the Dynamic primary identifier for the account object is preserved on a separate table from the Account table.
<br> <img src="./images/Identification-pattern-instance1.jpg" align="center" width=85% height=85%> <br>
#### Example 2
In this hypothetical scenario, the data team want to migrate data. In inserting data, new primary keys are generated, but the data team wants to preserve the Ids used on the old system.  <br>
Therefore, these legacy identifiers are preserved separately from the Asset table. <br>
<br> <img src="./images/Identification-pattern-instance2.jpg" align="center" width=85% height=85%> <br>
#### Example 3
In this hypothetical scenario, some customers send us information about Assets. These are attributes that they use as identifiers. Our customers want to use these details to find an Asset in our applications using their attributes on our web page.  <br>
Therefore, these external identifiers are preserved separately from the Asset table. <br>
You can see the External Identifier pattern in this document if you want more information. <br>
## References
<br> <br><br>
# External Identifier
## Category
## Description
This is a specialisation of the Identification pattern. The external identifier is a surrogate identification of a business entity. <br>
This design pattern is for when a system needs to share a business entity’s id with an external system.  <br>
The internal ids used in a system must not be shared with any external party for many reasons.  <br>
. Privacy: Internal ids can be sensitive information, and sharing these with external parties could breach privacy concerns.  <br>
. Security: Third parties could lose control of shared ids given in confidence. The leaking of identification information can be used for code injection attacks.  <br>
. Operations Suppose that a system shares internal ids. It is constraining the possibility of refactoring the database in the future because it will affect the third parties holding the ids pointing to records that may not exist anymore.  The solution for this hypothetical problem consists of providing external ids to external parties; these will be immutable of any internal refactoring. So that the external reference is kept intact and all systems interoperable.   <br>
For example, if a system needs to consolidate repeated records, it will require some merging records so that all internal ids will be affected. Still, the third parties will not be affected because they will keep referring to a ‘valid’ record, nevertheless.    <br>
Therefore, it is recommended that business entities' Ids that need to be shared with external parties use a surrogate id designed especially for the occasion.  <br>
### Sample Scenario 1
The following diagram represents the time when a company shares information about its assets with another company <br><br>
<br><img src="./images/External-identifier1.jpg" align="center" width=85% height=85%> <br> <br>
The following diagram represents when the company merges the records, which does not affect the third party.<br><br>
<br><img src="./images/External-identifier2.jpg" align="center" width=85% height=85%> <br> <br> 
### Sample Scenario 2
The diagram below represents the anti-pattern when external identifiers are embedded in the main business entity table. <br>
Instance diagram - Individual - driver's license - embedded. <br>
<br><img src="./images/External-identifier3.jpg" align="center" width=50% height=50%> <br> <br> 
The diagram below represents the correct pattern when external identifiers are stored in a separate table from the main business entity table. <br>
Instance diagram - Individual - driver's license - as Identification table apart.<br> 
<br><img src="./images/External-identifier4.jpg" align="center" width=70% height=70%> <br> <br> 
## References
<br> <br> <br> 
# Unique Id – Short UUID
## Category
## Description
In the case of Resource-ids and other cases, it may be required to generate a short UUID.  <br>
Short UUIDs can be generated by scratch or derived from Long UUIDs. <br>
## References
**JS UUId library**<br>
This library generates different types of unique IDs. There is a variation where it can generate only numbers. The numbers need to be unique and at least 19 digits long. <br>
https://www.npmjs.com/package/uuid <br>
**Short UUIDs**<br>
This library generates a short version of UUIDs.  <br>
https://shortunique.id/ <br>
**Long UUID to Short UUIDs conversion**<br>
This library converts the Long version of UUIDs to short UUIDs. <br>
https://www.shortuuid.com/ <br>
<br> <br> <br>
# Resource-Id – (restful API URI)
## Category
## Description
This type of Identifier is used to identify resources but needs to be human-readable. <br>
Business Entities designed to be exposed through restful API should have a human-readable identification number to be used as part of restful API URLs. <br>
Therefore, they will be used for bookmarking a business entity in a browser.  <br>
So that when the user clicks the bookmark, an application will open the resource and display it accordingly. The scenario is when someone needs to send the link to an invoice; then, it can copy and paste the invoice URL and send it to a client.  <br>
For example: <br>
/myapplication.com/customer/``<customer_resource-Id>``/account/``<account_resource-Id>``/ <br>
 <br>
### Requirements
This resource-Id can be designed in multiple ways. These resource Ids are going to be exposed to end users, including developers; the only real requirements for this resource-Id are: <br>
. They need to be unique.  <br>
. They need to be as short as possible.  <br>
. Need to be as human-friendly as possible.  <br>
 <br>
### Design
#### Alternative 1 - GUID
As it was proposed in the Primary Key design principle using GUID. The question is if the GUID used as the primary key makes a good resource-Id. <br>
That will be of the format:<br>
/customer/``A2eXh-HBwHj-Gd04t-zezmP-ojU65``/account/``A2eXh-HBwHj-Gd04t-zezmP-ojU65``<br>
This option is not good; it is just listed here as an anti-pattern because the GUID is meant to be private and only for the internal functioning of the table ids and foreign keys. <br>
Exposing the primary key of databases to external parties can be a security concern and creates a surface attack on hackers for code injection through APIs.<br>
In addition, the GUIDs are not human-friendly ids; they are too long and complex.<br>
#### Alternative 2 – Natural Keys and Business Keys
These seem to be a good fit because they are human-friendly. <br>
However, the Natural Keys and Business Keys are not even proposed for Primary Key because they have the potential problem that they could not be unique. <br>
Therefore, they could create unsolvable collisions if used as unequivocal identifiers for business entities. <br>
So, they cannot fulfil the unique requirement.<br>
####  Alternative 3 – Short UUID
That will be of the format (alphanumeric or numeric):<br>
. /customer/``hEUhaW``/account/``C6gsiW`` <br>
. /customer/``127392818``/account/``292839467`` <br>
Using a short id is possible because short UUIDs are more human-friendly than having GUIDs. <br>
####  Alternative 4 – Mnemotechnical hash ids
That will be of the format (3m-australia-pty-ltd): <br>
. /customer/``3m-4u57r4114-p7y-17D``/account/``4cc-80929C`` <br>
Using Mnemotechnical hash ids is possible because they are relatively short And human-friendly. <br>
 <br>
##### SQL design
. Field name: ``resourceId`` or ``resourceUUID``  <br>
. The SQL field will be ``varchar (50)``  <br>
. It will be a type of ``Unique`` field.  <br>
. It should be indexed because it will be used in “SQL ``where`` statements.”  <br>
. The resourceId field will not be used for foreign keys. <br>
 <br>
##### For eventual consistency scenarios
Other microservices and systems that store references to these business entities that contain resourceId should not store or refer to entities by using this resourceId but use the GUID instead. <br>
This type of Ids have similarities with the Custom Human-readable Id. <br>
## References
<br> <br> <br> 
# Mnemotechnical hash ids
## Category
## Description
Also, there is another interesting library that generates hash ids from text. The hash id has a mnemotechnical relationship with the original (the same way custom car plates are designed). These are implemented by Hash Id org.<br>
### Example
<br>

| #	| Original  | Hashed |
| --- | --- | --- |
|1|		Nano-ID	|N``4``n``0``-``1``D|
|2|		Separators|	``53``p``4``r``470``r``5``|
|3|		Read-more|	R``34``d-m``0``r3|
|4|		small-open-source-library|	``5``m``411``-``0``p3n-``50``urc``3``-``11``br``4``ry|
|5|		brute-force-attack|	bru``73``-f``0``rc``3``-``4774``ck|
|6|		StackOverflow|	``574``ck``0``v``3``rf``10``w|
<br>

### Caution about possible Collisions
. The generation of shorter ids could lead to more probability of collisions. <br>
. So there should be a logic that catches possible collisions and try to generate a new short id. <br>
. This must be implemented in a separate field from the Primary Key Long UUIDs; in this case, the field should have the SQL property of Unique.<br>
<br>
### Scenarios
This type of Ids has similarities with <br>
. Custom Human-readable Id <br>
. Resource Id <br>
 <br>
They can be adopted for Restful APIs as Resource ids. For example: <br>
From this type of ids:<br>
/customer/``Contoso``/account/``npb5YA4Dep3yMqAP7rRN``<br>
<br>
Transformed to:<br>
/customer/``C0n7050``/account/``npb5Y44D3p3yMq4P7rRN``
<br>
### Randomness and Uniqueness
If extra randomness is required, each letter transformation can also be transformed randomly.  <br>
This is the case when these types of hash-ids are used for resource ids; the field must be marked as Unique. This can bring a problem when using soft-delete, and then a new record is created with the same resource ids that already exist, but it is only soft-deleted in the table. To circumvent this problem, the hash id function can be extended and randomised for each letter. <br>
For example, consider applying the transformation from uppercase to lowercase or vice versa in all letters and randomly apply the transformation pattern from a letter to numbers. <br>
So, if the same name is to be created again in the database to avoid collision (because of the Unique property of the field), it will generate one of the possible variations of the hashed name. <br>
For example: <br>

| Variation #	| Original  | Hashed |
| --- | --- | --- |
|1| Contoso | C``0``n``7050`` |
|2| Contoso | ``c0``n``7``o``5``o |
|3| Contoso | Co``N7O5``o |
|4| Contoso | ``c``o``N``to``S0`` |
<br>

## References
Hash Id Organisation <br>
https://hashids.org/
<br> <br> <br> 

# Custom Human-Readable-id
## Category
## Description
Some systems may require creating business entities with custom human-readable or human-friendly Ids. <br>
Many systems allow the User to write the custom id, or the system can create them by inferring the name from another field. However, in most cases, the system allows this custom id to be editable, and the User can overwrite the proposed generated id.<br>
These types of IDs have similarities with the Resource-Id and the Short-UUID. <br>
See the example below, where the user can create a custom "Customer Id". The custom id is proposed from the field above, but the User can edit it.<br>
<br><img src="./images/Custom-Human-Readable-id1.jpg" align="center" width=50% height=50%> <br> <br> 
### Important
. Because the field is created by hand, these Ids can lead to collisions. <br>
. So there should be a logic that catches possible collisions and propose a variant of the same id. For example, adding a random two-digit number.<br>
For example: <br>
|  #	| URI |
| --- | --- |
|Original| /system/customer/``3m-australia-pty-ltd``   |
|Modified| /system/customer/``3m-australia-pty-ltd-01``  |
   
. These Custom Ids (human-friendly) cannot be a Primary Key. They should be in a separate field with the table, and it is recommended to have the SQL property ``Unique``.<br>
## References
<br> <br> <br> 
# Foreign Key
## Category
## Description
This topic of Foreign Keys only refers to a naming convention when designing the Physical data model.<br>
It is important to adopt a convention and be consistent with Foreign Keys.<br>
They follow the name convention: <br>
``FK`` (Prefix) +``_`` +``<The name of the entity>``+``_`` +``<The name of the foreign entity>`` + ``Id``<br>
For example: <br>
ClientCompany has a foreign key to the Tenant table: ``FK_ClientCompany_TenantId``<br>
Note: Assuming that it has been adopted, the Primary Key design proposed as the UUID or GUID, then the foreign key should only have this primary key from other tables. Otherwise, the database is not able to keep SQL referential integrity.<br><br>
<br><img src="./images/ForeignKey1.jpg" align="center" width=80% height=80%> <br> <br> 
<br><img src="./images/ForeignKey2.jpg" align="center" width=50% height=50%> <br> <br> 
## References
Referential Integrity Wikipedia<br>
https://en.wikipedia.org/wiki/Referential_integrity<br>
Basis of Referential Integrity in Databases <br>
https://www.w3resource.com/sql/joins/joining-tables-through-referential-integrity.php
<br> <br> <br> 
# Join table name
## Category
## Description
This is a naming convention about how to name these tables. These types of tables resolve N-to-N relationships.
They follow the name convention: <br>
``<The name of the first entity>``+``_`` +``<The name of the second entity>`` +``_`` +``Join``<br>
For example: <br>
A hypothetical scenario where a customer has many Orders, and the Order can be an aggregation from multiple Customers: ``Order_Customer_Join``<br>
For example<br>
<br><img src="./images/JoinTable1.jpg" align="center" width=80% height=80%> <br> <br> 
Note: If the table names are long, then the concatenation of table names plus the “Join” can be too long. So, in this case, you can apply some abbreviations to the names to make it practical.
## References
<br> <br> <br> 
# Audit fields
## Category
## Description
This is another of the most used patterns. Any system that has "Auditablity" as a non-functional requirement must address it and have some implementation.<br>
In this pattern, it is proposed the data modelling for these audit fields. <br>
In this design, fields are updated every time the record is created (only once); they are modified or deleted (only once).<br> 
The limitation of this design is that these fields alone cannot keep the history of changes.<br>
So, every time a record is updated, the ``auditModifiedBy`` and ``auditModifiedDateTime`` will be overwritten.<br>
Therefore, for this design to be effective must be complemented with another design which is the extraction of the updated fields every time the record is changed.<br>
In large organisations, usually, there are processes that capture individual changes in application databases and send them to Data Warehouse, so when a row is updated, we can do it safely because the system is guaranteed that a copy is already made. The update will not cause any data to be lost.<br>

|#| Field Name|	Type|	Description|
| --- | --- | --- | --- |
|1|	auditCreatedDateTime	|	dateTime	|Date time when the business entity being audited was created.|
|2|	auditCreatedBy	|		varchar(50)	|User id in the session when the business entity was created.|
|3|	auditDeletedDateTime|		dateTime	|Date time when the business entity being audited was deleted.|
|4|	auditDeletedBy|			varchar(50)	|User id in the session when the business entity was deleted.|
|4|	auditModifiedDateTime|		dateTime	|Date time when the business entity being audited was modified.|
|6|	auditModifiedBy|		varchar(50)	|User id in the session when the business entity was modified.|

In addition, there may be convenient to have extra fields needed to support the Operation teams in correlating transactions among systems when performing audits<br>

|#| Field Name|	Type|	Description|
| --- | --- | --- | --- |
|1|	auditInternalCorrelationId|	varchar(50)	|This is the ``correlationId``. The internal correlation id is a field used for end-to-end traceability among all the components of our distributed architecture. It allows the implementation of the observability capability by the DevOps team. |
|2|	auditExternalCorrelationId|	varchar(50)	|This is the ``externalCorrelationId``. The external correlation id is a field that external organisations create. For example, Customers send data in B2B interfaces along with their correlationId, which in our systems will be an externalCorrelationId. Then, the Customers log in to our website and can track these transactions using the sent correlationsIds embedded with our processes.|
<br>

### Logical deletion
This design also overlaps with the Logical Delete pattern. The Logical delete allows the logical deletion of rows without removing them from the database. <br>
This is by filling in the ``auditDeletedDateTime``. So when the application finds that ``auditDeletedDateTime`` is not blank, it must assume that it has been deleted.<br>

### Historical changes
A frequent question raised by developers about this pattern is that when there are multiple updates, the tables only preserve the latest change, and the audit fields are overwritten, losing the previous information on the ``auditModifiedDateTime`` and ``auditModifiedBy`` fields. <br>
So, the questions are:<br>
Does this pattern keep a version of each record before changing or after changing them?<br>
The answer is no and no. This pattern only keeps the metadata about the event, but it does not solve keeping the historical changes.<br>

### Change Data Capture (CDC)
To preserve all the changes in the database, different alternatives depending on the database and requirements. <br>
But in all cases should consider keeping a copy of the existing record before overwriting it. <br>
One of the most used patterns is the Change Data Capture, which allows monitoring and tracking the change log in the database, extracting the data and publishing these changes as events to Kafka or elsewhere. So, every change occurred in a business domain, at the same time, is saved in the database and is also sent as an event to the eco-system. <br>
The Kafka platform is a technology that keeps all the events as records in a database, so it is possible to reconstruct all the changes that occurred through time.<br>
Another alternative, if working with a cloud PaaS database, is to enable the track changes. So, every change is preserved.<br>
Any method implemented must be verified and tested, and operations must know how to use it.<br>
## References
Backend Side design <br>
CQRS (Command and Query Responsibility Segregation) can complement Eventual Consistency. <br>
Eventual Consistency <br>
https://fauna.com/blog/why-strong-consistency-with-event-driven <br>
CQRS <br>
https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs <br>
Outbox pattern  <br>
https://debezium.io  <br>
Azure SQL Track data changes <br>
https://learn.microsoft.com/en-us/sql/relational-databases/track-changes/track-data-changes-sql-server
Automerge <br>
Client Side design <br>
It can be seen as the implementation of the Automerge protocol <br>
https://github.com/automerge/automerge <br>
<br> <br> <br> 
# Correlation-Id  and Trace-Id
## Category
## Description
Important metadata needs to be considered for the Observability capability implementation. It allows debuggability, operability and audit-ability of a system.<br>
### Correlation ID
It is an end-to-end identifier of a business interaction that can comprise multiple events and actions in several systems. It is an  additional identification of a client application so that the business interaction can be traced end-to-end from the original request, the logic, the access to data, and the consequent response. <br>
The name convention: ``correlationId`` <br>
. The correlation ID is used to correlate related events or transactions across different components or services in a distributed system.<br>
. It helps track a single logical flow of a request or operation as it traverses various components or microservices.<br>
. Correlation ID remains the same throughout the entire request/response cycle, allowing all logs and events associated with a specific operation to be linked together.<br>
. It is typically set at the system's entry point (e.g., an API gateway or load balancer) and propagated to downstream components.<br>
. Correlation ID can be used for troubleshooting, debugging, performance analysis, and auditing.<br>
<br>
There are two types of correlation id:
#### CorrelationId (internal)
It is a Unique identifier. <br> 
The Client or Producer adds it to the requests, and the same value is populated to all the consequent messages' payloads created as a consequence of the initial message processing so that it is possible to track the event chain.<br> 
#### ExternalCorrelationId (External)
It is a Unique identifier. <br> 
It is generated by third parties when sending a request. The Client or Producer adds this to the requests, and the same value is populated to all the consequent messages' payloads created as a consequence of the initial message processing so that it is possible to track the event chain.<br> 
### Trace Id 
It is usually implemented in the context of correlating logging entries in a system.<br>
The name convention: ``traceId`` <br>
. Trace ID is used to trace the path of a request as it travels through multiple services or components in a system. (The system can be monolithic or distributed, but that does not change the nature of what is considered a system).<br>
. It provides a unique identifier for each individual request or transaction.<br>
. Trace ID is generated at the system's entry point and passed along with the request as it propagates through different services.<br>
. Each service or component adds its trace ID to the request, creating a chain of trace IDs representing the entire request path.<br>
. Trace ID helps understand a request's end-to-end flow and performance characteristics, including any bottlenecks or latency introduced by specific services.<br>
. It is commonly used in distributed tracing systems to analyze the performance and dependencies of services within a distributed architecture.<br>
## References
<br> <br> <br> 

# Temporal Object (or Versioned Object pattern)
This pattern is used for the scenario where an entity needs to represent a single instance for some consumers (showing continuity), preserve all the changes, and track all the versions for others, therefore being several versions.<br>
<br>
*“There are times when you like to think of an object having temporal properties, but others when you think of the object itself as temporal. A good example is a contract that goes through a series of amendments. You can think of each amendment as a new contract with new terms, yet you can also think of them as versions of the same contract.” “You might also provide direct access to the versions for reporting purposes. And also provide access to the one version showing the continuity face.“* <br>
Martin Fowler - March 2004 <br>
<br>
One concrete implementation of this pattern is the bi-temporal model used in the Land Administration domain for preserving all the history of owners (Land Registration Title holders) over time. But only one is the active one. The rest of the property title owners have a temporal reference from when to when they were the active title holder of the property, therefore the “active one”. <br>
Generally, this pattern requires that the old data is not required to be removed from the same database or even the same table. <br>
So, the historical data must not be removed or deleted from the application's transactional database. This implicitly assumes that the storage and management of those old records will not compromise or interfere with the main purpose of the online database. <br>
The bi-temporal design addresses the different scenarios and dimensions applicable for capturing the changes in the definition of land and parcels. So the cadastral database must have a temporal aspect recorded, where the changes are recorded using the ”versioned object” pattern. The times recorded in the model are ``ValidTime`` and ``TransactionTime`` referred to as the bi-temporal model.  <br>
. ``Valid_time`` (Start, End): It is the period when a fact is true in the real world. <br>
. ``Transaction_time`` (Start, End): It is when a fact is recorded in the database. <br>
In addition, there is an extra one: <br>
. ``Decision_time`` (Start, End): This is an extra dimension to the abovementioned main two. It is the time at which the decision was made about the fact.
 <br>
Note: some commercial databases already provide features to make tables “bi-temporal”. In the case of PostgreSQL, it does not have it as a built-in capability. But several enhancement packages can be used that will provide this capability.  <br>
<br>
Example: <br>
This example is inspired by the Codeproject-Bitemporal-Database-Table-Design-The-Basics article. Please take a look at the reference below.
This is a regular Product table:<br>

| productId | Name | Price_USD |
| --- | --- | --- |
|430bf27a3a5c|	Eggs	|1.20|
|a303a8269976|	Milk	|0.45|
|faa9eb81e162|	Bread	|0.30|
<br>
This is the Product table with the ``Valid_time`` dimension.<br>

|productId|	versionId|	Name|	Price_USD|	ValidFrom|  	ValidTo|
| --- | --- | --- | --- | --- | --- |
|430bf27a3a5c|	b4a8f53f7185|	Eggs|	1.20|	20/01/2006|	13/06/2006|
|430bf27a3a5c|	8ce5f997507e|	Eggs|	1.25|	``13/06/2006``|	``31/12/9999``|
|a303a8269976|	61ac4228f9e3|	Milk| 	0.45|	20/01/2006|	01/01/2007|
|faa9eb81e162|	d26c89e922aa|	Bread| 	0.28|	18/06/2005|	20/01/2006|
|faa9eb81e162|	44d37fa3fc2c|	Bread|	0.30|	``20/01/2006``|	``31/12/9999``|

Note the following:<br>
. The Milk does not have a current active price. This means that the Milk is not being sold any more. <br>
. The lines with the ``ValidFrom``and ``ValidTo`` in bold are the current active rows for these items.<br>
. The productId is not unique. So, another strategy must be designed for the foreign keys to this table. Or productId may be filled only when the row is the active one. But in this case, it cannot be nominated as the SQL primary key because SQL primary keys cannot have empty values.<br>

## References
Codeproject-Bitemporal-Database-Table-Design-The-Basics article <br>
https://www.codeproject.com/Articles/17637/Bitemporal-Database-Table-Design-The-Basics <br>
Martin Fowler Temporal Object, March 2004  <br>
https://martinfowler.com/eaaDev/TemporalObject.html <br>
<br> <br> <br> 
# Reference Data
## Category
## Description
These are the values that are used for displaying and selecting preconceived values:<br>
. Categories<br>
. Types<br>
. Class<br>
. etc<br>
<br>
It is advisable to adopt a naming convention for this type of table. <br> 
Then, follow this naming convention: <br>
``<Name of the data set>`` +``_`` + ``List``(Suffix)<br>
For example:<br>
``CountryCode_List``<br>
<br>
**Design**<br>
Usually, the reference data will consist of: <br>
. Code<br>
. Display name<br>
<br>
**Caution** <br>
One particularity of the Reference Data is that the values are never removed completely from the tables. These are deprecated, meaning they will not be available for displaying and selecting new datasets. This is because the Reference Data is usually used as a Foreign Key. Therefore, if they are removed, they could cause SQL violations to be triggered or data inconsistencies.<br>
<br>
**Others**<br>
In UML, usually, this type of data is referred to as Enum literals ``Enum`` (UML Class diagrams)<br>
## References
<br> <br> <br> 

# ISO Codes
## Category
## Description
These must be adopted when possible as reference data. Many of these can be found on Wikipedia. <br> 
<br> 
For example: <br> 
. Country Code <br> 
. Country calling code prefix
. Human sexes classification international standard <br> 
. etc <br> 

## References
Country Code <br> 
https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes <br> 
Country calling code prefix  <br> 
https://en.wikipedia.org/wiki/List_of_country_calling_codes  <br> 
Human sexes classification international standard <br> 
https://en.wikipedia.org/wiki/ISO/IEC_5218 <br> 
<br> <br> <br> 

# Watermark (design pattern)
## Category
## Description
This pattern is useful for data processing scenarios. For example, in data import/ETL scenarios, often it is needed to track the last imported set of data so that the import or synchronisation process can be: <br> 
. Resumed (from last imported date). Only considering the new records.<br> 
. Re-executed (from last imported date). Only considering records from a saved date.<br> 
<br> 
Commercial software usually implements these using a design pattern called “Watermark”.<br> 
The watermark is implemented as a table containing metadata about the import or synchronisation processes.<br> 
The ``startRecordDateTime`` and ``endRecordDateTime`` are filled with one of the audit fields, depending on what type of operation was: ``createdDateTime``, ``updatedDateTime`` or ``deletedDateTime``.<br> 
<br> 
Watermark Table design<br> 
|#|	Field Name|	Type|	Description|
| --- | --- | --- | --- |
|1|	watermarkId|	Unique Identifier|	Unique identifier. UUID.|
|2|	watermarkName|	String|	Name of the watermark for easy query.|
|3|	processId|	Unique Identifier|	Unique identifier. UUID.|
|4|	processName|	String|	Name of the batch process for easy query.|
|5|	sourceTableName|	String|	Name of the table, source of the information.|
|6|	destinationTableName|	String|	Name of the table, destination of the information.|
|7|	startRecordDateTime|	DateTime|	The date-time of the first record processed. (Assuming that the process will process the records incrementally by time. - The ones that were modified first will be processed first-)|
|8|	endRecordDateTime|	DateTime|	The date time of the first record processed. (Assuming that the process will process the records incrementally by time. - The ones that were modified first will be processed first-)|


## References
More about the design watermark pattern on the following links <br>
Incrementally load data from a source data store to a destination data store <br>
https://docs.microsoft.com/en-us/azure/data-factory/tutorial-incremental-copy-overview  <br>
Incrementally load data from Azure SQL Database to Azure Blob storage using the Azure portal  <br>
https://docs.microsoft.com/en-us/azure/data-factory/tutorial-incremental-copy-portal  <br>
Incremental or High-Water Mark data Loading  <br>
https://documentation.matillion.com/docs/2506598  <br>
<br> <br> <br> 

# Soft Delete
## Category
## Description
The audit field “auditDeletedDatetime” can be used as the Boolean field that indicates if the record has been soft deleted.<br>
Some ORM (Object Relationship Mapping) libraries allow customising which field will be used by the model to differentiate soft deleted records.<br>
It's worthwhile to investigate how the ORM library settings work to be able to customise the soft-delete field.<br>

**Caution**<br>
When adopting the soft delete, after a record is soft-deleted, there could be business scenarios where the user creates a new record with the same business entity can cause problems. <br>
The Primary key will not be the same (If using GUID as the primary Key); however, if the entity has other fields and identifiers with the SQL ``Unique`` property, then the database will not allow the creation of this new field. This can happen with the Business Key, Natural Key, Resource Id, Mnemotechnical hash Ids, etc.<br>

## References
TypeORM Soft Delete field setting features <br>
https://github.com/typeorm/typeorm/pull/5034 <br> 
https://typeorm.io/#/decorator-reference/deletedatecolumn <br>
https://typeorm.io/#/delete-query-builder <br>
Sequelize ORM Soft Deleted <br>
https://www.npmjs.com/package/sequelize-soft-delete <br>
<br> <br> <br> 

# InvolvementRoleAssociation
## Category
## Description
This pattern is one of the modelling best practices of Industry models that deal with complex business entity relationships.  <br>
It represents an association/relationship between two business entities. (See Party modelling section of this document).  <br>
The ``InvolvementRoleAssociation`` relationship represents more than a mere ``Join`` table because it has some extra attributes that are used to describe the association in the following ways: <br>
. Quantify  <br> 
. Qualify  <br>
. Lifecycle (Status, start and end date)  <br>
 <br>
For example: 
Instead of modelling the relationship as a Join 
<br><img src="./images/InvolvementRoleAssociation1.jpg" align="center" width=90% height=90%> <br> <br> 
<br>
The relationship is complex with more metadata and is an InvolvementRoleAssociation
<br><img src="./images/InvolvementRoleAssociation2.jpg" align="center" width=90% height=90%> <br> <br> 
<br>
For example: <br>
|#|	Field Name|	Type|	Description|
| --- | --- | --- | --- |
|1| InvolvementRoleAssociationProductOwnerId|Unique Identifier|	Unique identifier. UUID.|
|2| Name | varchar(50)|Name of the InvolvementRoleAssociation
|3|Description |String|Description of the InvolvementRoleAssociation|
|4|Type|varchar(50) | Qualifier. Type or relationship is. For example: 'Legal', 'Binding', 'Formal', 'Professional'etc|
|5|Start_Date|DateTime| From what date time this relationship is active|
|6| End_Date |DateTime| Until what date time this relationship is active|
|7| Status |varchar(50)| What status this relationship is. For example, 'Acvtive', 'Suspended', 'Dormant', etc.|
|8|Context | varchar(50)|Qualifier. Names of the context where this association can be used. For example, 'Online', 'Back-office', 'Chatboot', etc |
|9| Category |varchar(50)| Qualifier. For example: 'Social Media', 'Email Marketing', 'Online Advertising', 'Online Forums', etc.|
|10| Direction |varchar(50)| What direction this relationship must be read. For example: 'Product-to-User', 'User-to-Product', 'Bidirectional'|
 <br>
Other attributes for more complex scenarios where the relationship belongs to another system, so a new set of attributes may be convenient to be stored for reference:<br>

|#|	Field Name|	Type|	Description|
| --- | --- | --- | --- |
|1|Alias|varchar(50)|Other names applicable to the relationship. For example: "Purchased Subscription"|
|2|ApplicationOriginName |varchar(50)| This relationship is mastered in another system, and this system only has a copy. Application name|
|3|ApplicationOriginValue |varchar(50)|This relationship is mastered in another system, and this system only has a copy. Relationship original name on the external Application. For example: 'Anual Subscription'|
## References
<br> <br> <br> 

# Privacy fields
## Category
## Description
This topic deals with identifying data that could be under privacy regulations and how to address the security requirements from the data at rest perspective. <br> 
This is part of a broader Privacy solution that addresses the requirements mandated by Privacy laws in jurisdictions. <br> 
The following Business Entities and attributes should be treated under privacy policies.<br> 

### Individual Identifiers
. Medicare number/Social Security number <br> 
. Passport number <br> 
. Driver’s license number <br> 
. etc. Any other government-issued id  <br> 

### Individual personal attributes
. Full name <br> 
. Date of birth <br> 
. Age <br> 
. Home address <br> 
. Email address <br> 
. Sexual orientation  <br> 
. Ethnicity  <br> 
. etc <br> 

### Individual financial attributes
. Credit card numbers <br> 
. Bank account <br> 
. Payment platform token identifiers <br> 
. File Tax Number <br> 
. etc. <br> 

### Online identifiers
. Login details (username)
. etc <br> 

### Media identifiers
. Processor or device serial number  <br> 
. Media access control (MAC)  <br> 
. Internet Protocol (IP) address  <br> 
. Device IDs  <br> 
. Location  <br> 
. Cookies  <br> 

### Solution
Generally, we talk about encryption at different levels.  <br> 
 <br> 
***Database encryption*** <br> 
Depending on what databases using, there could be different encryption mechanisms and granularity options.  <br> 
. Partition  <br> 
. Database  <br> 
. Table  <br> 
. Column  <br> 
<br> 
***Application encryption*** <br> 
Data encryption and obfuscation can be seen as an array of options that can be applied to different scenarios. <br> 
There are different types of data obfuscation, encryption, masking, anonymisation and tokenization. <br> 
Using any particular type for each security scenario is not the objective of this part of the solution. Please look at the Anonymisation, Tokenisation and Masking section of this document.
## References
<br> <br> <br> 

# Anonymisation, Tokenisation and Masking
## Category
## Description
### Anonymisation
This is a way of replacing the original values with data that provide a realistic feeling. The anonymised data can be used as a replacement for the original data in many scenarios. <br> 
For example, we can recreate attributes by replacing the original value and respecting the format. So instead of exposing real names, IDs, telephone numbers, addresses, and postcodes, we can use the anonymised data for testing, for example.<br> 
Regarding the production environment, anonymisation can deal with sensitive individual information at rest. So that when it is recorded in the database, it will not be practical, even if someone internally or externally to the company makes a dump of the database and copies these values.<br> 
 <br>
***Anonymisation Example*** <br>

|#|	Original|	Anonymised data|
| --- | --- | --- |
|1| John Peter Salisbury		| ``Csmu Lzwly Qnyiyfoqw``	|
|2| john.peter.salisbury@example.com	| ``rclp.uzlzd.yymrjceym@cyaiert.wor`` |
|3| 1279 Basil Road			| ``2026 Icjjo Good``	|
|4| San Francisco			| ``Gmd Vcpwayjpq``	|
|5| Łódż, Polska			| ``Ĭãsṻ, Yleivd`` 	|
|5| +31 (0) 20 123 234 56		| ``+80 (4) 98 803 550 44``	 |
<br>

### Tokenization
It transforms a piece of data into a string of characters called a token.<br> 
This new token does not directly relate to the original piece of data.<br> 
Tokens are used as a reference to the original data. The data cannot be read in plain sight, but the token is used instead of the original. <br> 
Tokenisation does not use a Cipher program, and Tokenisation is a process that relies on storage where the data is translated from one to the other. This repository, called 'Token vault', is usually provisioned from cloud providers as a PaaS. <br>
This database stores the relationship between the original data (sensitive) value and the new replacement, called a token. <br>
The real data in the vault is then secured by encryption, and no human access is ever allowed. <br>
The original value stored in the 'Token Vault' is encrypted. It cannot be accessed by any means, only by the API provided that never provides the functionality of de-tokenising the original data. <br>
The token value should be adopted as the only way of identifying the substituted original data.  <br>
For example, one of the most famous data attributes that are subject to Tokenisation in the Payment industry is the Credit Card PAN (Primary Account Number), which is replaced by a token (of the same data format) for companies, platforms and business entities be subject to PCI compliance.  <br>
Tokens also secure other types of sensitive or personally identifiable information. This includes data like Passports, Driver's Licenses, and Medicare cards.  <br>
In addition, it could also be used for Telephone numbers, email addresses, etc. This depends on the strategy chosen for each attribute.  <br>
<br>

 ***Tokenisation example*** <br>
|#| Original| Tokenized data|
| --- | --- | --- |
|1| 38338383 |``93746385`` |
<br>

### Masking
This is a way of showing partially the original value. <br>
Masking example with different masking strategies<br>
<br>
 ***Masking example*** <br>
|#|	Original|	Masked data|
| --- | --- | --- |
|1| foobaruser@emailservice.com |``fo********@emailservice.com`` |
|2| foobaruser@emailservice.com | ``foobaruser@************.com`` |
|3|foobaruser@emailservice.com | ``foob*******************.com`` |
<br>

### Implementation strategies
#### In a single Application, only
This could be one implementation strategy when developing a simple application. <br>
In TypeORM, no built-in decorator is specifically designed to implement automatic field encoding and decoding.  <br>
However, you can achieve this functionality by combining custom getter and setter methods with existing decorators. <br>
The ``BeforeInsert`` and ``BeforeUpdate`` decorators are applied to the ``customEncodeField`` method, ensuring the field is encoded before saving. <br>
The cryptographic key must be stored in a proper secure key vault; only encrypted and signed libraries can access it.<br>
<br>
Another alternative could be using database features if available.

#### Across Applications
The same algorithm must be applied for this scenario when encoding and decoding data across applications. <br>
Therefore, a common strategy for applications that share encoded data must be used. This can be neutral centralised API or common libraries embedded in applications. <br> 
When opting for APIs, there should be careful design for high performance, throughput, and availability. Without these, no data can be transformed in real-time.<br>
The cryptographic key must be stored in a proper secure key vault; only encrypted and signed libraries can access it.<br>
<br>

## References
Anonymisation tool - js-data-anonymizer <br>
https://github.com/joostvunderink/js-data-anonymizer  <br>
Tokenisation tool - tokenex  <br> 
https://www.npmjs.com/package/tokenex   <br>
Masking tool - simple-data-anonymizer  <br>
https://www.npmjs.com/package/simple-data-anonymizer    <br>
<br> <br> <br> 

# Multitenancy design
## Category
## Description
The Multi-tenancy design and implementation can have multiple implications on data modelling depending on the requirements. The architecture design can be only to the scope of an application where all the Tenants are running simultaneously, or it can go further to the different levels of the technology stack, for example, platforms, containers, networking, infrastructure, etc. Therefore, affects several layers in the architecture. <br>
So, in developing a generic data design guidance for multi-tenancy scenarios, I postulate two scenarios that can be taken as a reference from the data design perspective. These are Application database design and a more complex Infrastructure and platform design, where tenants need to be segregated not only from the data perspective but also from the allocation of resources. So, briefly, I can mention the problem space for both scenarios:<br>
. For Applications supporting more than one Tenant. This has repercussions on Database design and also hosting names and API URLs.  <br>
. For the infrastructure hosting more than one tenant. This affects the whole stack, from network compartmentalisation to workload allocation and isolation, capping, etc. There are several models to follow depending on the occasion. More below. <br>
<br>

### Application Multi-tenant design
From the Application design perspective, the problem scope and narrowed to data design; the Application must understand and manage data from multiple Tenants without confusing or mixing it.  So, the simplest design strategy consists of adding a ``TenantId`` qualifier to all rows to identify the data owner. <br>
To complete the idea, the TenantId implementation must also be embedded in the services and APIs exposed as interfaces, and it could also be considered having several permutations of the same client applications for the same backend instance. This is not so uncommon.<br>
If the application on the backend consists of multiple deployable units, such as microservices domains, then the TenantId must be implemented in all the databases if there is more than one. <br>
#### Application Multi-tenant database
These repositories can be designed with two different approaches, and I will use the Domain-Driven Design nomenclature to explain them. <br>
. A TenantId field can be placed at the aggregate root level only. <br>
. A TenantId field can be placed in every database table, not just the domain root entity. Despite the data evident data redundancy, this option is valid because it clarifies the data models and avoids making silly mistakes when querying or manipulating datasets.<br>
<br>
About how to propagate the TenantIds in a distributed environment, it could be assumed that the ``TenantId`` concept fit into the reference data category. So, there should be a master table with all the reference data somewhere in the eco-system, and it will be used as the source of truth to propagate all new data or changes to any reference data dataset, to all microservices through the reference-data-propagation-rails whatever this has been decided to be. So, it could be treated as any other reference data set. <br>

### Infrastructure and platform  Multi-tenancy design
From the infrastructure design perspective, these designs have been evolving and being discussed on the open in later years. Building large scalable SaaS applications was only a thing of big players. But increasingly, more architects confront this scenario and develop patterns and best practices. So, I analysed the best articles available when writing this (cited below in the reference section, which is from Microsoft and other sources); these are the variations proposed.<br>
#### Infrastructure Multi-tenancy models
. Single-tenant deployments <br>
. Fully multitenant deployments <br>
. Vertically partitioned deployments <br>
. Horizontally partitioned deployments <br>
<br>
#### Tenant-catalogue database
From the data design perspective, they propose having a database with all Tenants' metadata. This database is named a Tenant-mapping repository or a Tenant-catalogue. <br>
The Tenants' metadata will consist of all the URI, URLs, physical addresses and service Ids assigned to each Tenant. This data can differ depending on the cloud provider and if using container platforms such as Kubernetes. <br> 
The idea is that architecture components can use this database as the source of truth to find the correct platform instances and services for a particular Tenant.<br>
Regarding the infrastructure design of this database, although it is possible to have a centralised database with all the Tenant metadata running on the infrastructure, this could slow down the performance significantly. So, to solve this potential bottleneck, the data must be propagated to the actual platforms to run independently without having to fetch the data. For example, Proxies, Kubernetes namespaces, Service Mesh configurations, Kafka Topic and partitions, and any other service or platform that requires it.  <br>
In addition, the architecture proposed relies on a CI-CD pipeline that provisions dynamically runtime environments to new Tenants.  <br>
Once the deployment of a new Tenant is triggered, a Dev-Ops pipeline starts, and the first thing it does is query the database to obtain all the information about the new Tenant. The Tenant information will include the tenant model (see above) to be used and how many components need to be provisioned for each case.  <br>
The CI-CD will translate this general information to a new physical environment for the Tenant. This process places static configurations at different stack levels to permit traffic to flow correctly for this Tenant. For example, in the API Gateway or Proxies, identifying the Tenant and routing the Tenant traffic to the correct Kubernetes Pod or Containerised application. So these Proxies know where to route requests from different Tenants without fetching any data from external sources.   <br>
During the deployment process, the CI-CD pipeline may collect new information about the services-ids assigned by the cloud providers to the new Tenant instances; these data must also be preserved in the Tenant-catalogue database; so the CI-CD pipeline will collect this data and update the Tenant-catalogue accordingly. <br>
<br>
#### Data isolation - Database sharding
In any of the two scenarios presented above for Applications and Infrastructure multi-tenancy, one of the concerns could be how to ensure data privacy and performance for each Tenant. This is data isolation by design. This can be achieved by:<br>
. Creating a dedicated database instance per tenant. <br>
. Creating a sharding instance per Tenant. <br>
In the dedicated database model, there should be a configuration of Tenant-Database-instance for the application to find the correct repository.<br>
In the sharding model, a Tenant or a group of Tenants will be placed in one of the database shards. In most cases, the sharding technology makes the sharding internal partition configurations transparent to the Application.<br>
Implementing the ``TenantId`` at the row level proposed can be helpful if it is a single database or helping the process of converting a single database to multiple shard partitions per Tenant.<br>
<br>

## References
Multi-tenancy models <br> 
Microsoft Tenancy models for a multitenant solution <br> 
https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/considerations/tenancy-models <br> 
Microsoft SaaS and multitenant solution architecture <br> 
https://learn.microsoft.com/en-us/azure/architecture/guide/saas/overview <br> 
Microsoft Multi-tenant SaaS database tenancy patterns <br> 
https://learn.microsoft.com/en-us/azure/azure-sql/database/saas-tenancy-app-design-patterns <br> 
Hassle-free multi-tenant K8S clusters management using Argo CD <br> 
https://blog.argoproj.io/hassle-free-multi-tenant-k8s-clusters-management-using-argo-cd-7dd35619046a  <br> 
Manage namespaces in multitenant clusters with Argo CD, Kustomize, and Helm <br> 
https://developers.redhat.com/articles/2022/04/13/manage-namespaces-multitenant-clusters-argo-cd-kustomize-and-helm  <br> 
Multi-tenancy authentication through Kong API Gateway  <br> 
https://medium.com/@cezarromaniuc/multi-tenancy-authentication-through-kong-api-gateway-e05e08308da8  <br> 
Multitenancy API design patterns  <br> 
https://medium.com/@vivekmadurai/multi-tenancy-in-rest-api-a570d728620c  <br> 
Multi-Tenant Architecture for Designing a SaaS Application  <br> 
https://relevant.software/blog/multi-tenant-architecture/  <br> 
<br> 
Database sharding   <br> 
Sharded Multi-Tenant Database using SQL Server Row-Level Security  <br> 
https://www.codeproject.com/Articles/5318079/Sharded-Multi-Tenant-Database-using-SQL-Server-Row  <br> 
Schema-based sharding for Postgres  <br> 
https://www.citusdata.com/blog/2023/07/18/citus-12-schema-based-sharding-for-postgres/  <br> 
<br> 
Cloud Service Ids   <br> 
AWS - Amazon Resource Names (ARNs)   <br> 
https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html  <br> 
Azure - Resource-id format  <br> 
https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-resource-group-and-subscription#changed-resource-id  <br> 
Kubernetes - Object Names and IDs  <br> 
https://kubernetes.io/docs/concepts/overview/working-with-objects/names/  <br> 
<br> 
<br> 

# Multi-Company design
## Category
## Description
Company is a concept related directly to the implementation of SaaS. The Company concept indicates which company uses an Application or is “hosted” in an Application. <br> 
It is a concept also related to Multi-Tenancy or extends the Multi-tenancy concept. <br> 
The scenario that best explains when this Company concept is needed is that an Organisation (a Tenant) subscribes to a SaaS service (a SaaS Application). Then, this Tenant organisation requires inviting users from other Companies under the parent Tenant organisation. <br> 
So, the Tenant and the Company are related concepts, both are organisations, but they are not at the same level, and therefore they are not the same and interchangeable concepts. There are multiple scenarios where this relationship Tenant-child-Company is required. For example, authorisation of users, managing licences, caping expenses, consolidating child Companies' bills to a single bill, etc. <br>
<br><img src="./images/Tenant-Company1.jpg" align="center" width=85% height=85%> <br> <br> 
<br>
The example below: <br> 
. The Tenant has multiple child Companies; all companies are invited to use an Application. <br>
. The users of the Companies are provided with access to the Application. <br>
<br><img src="./images/Tenant-Company-instance1.jpg" align="center" width=85% height=85%> <br> <br> 
<br>

The minimum implementation of the Company concept is to have the Unique identifier key named ``CompanyId``.
So, the CompanyId implementation in domains can be: <br>
. A field at the aggregate root level only. <br>
. A field in all tables, not just the domain root entity.  <br>
## References
<br> <br> 
<br> 
# Name conventions (Tables, Columns and others)
## Category
## Description

### name-casing practices
This is only a quick reference to possible name-casing practices:<br> 
|#|	Original data|	Description| Example |
| --- | --- | --- | --- |
|1|``UPPERCASE``|Simple uppercase|``NAME``|
|2|``lowercase``|Simple lowercase|``name``|
|3|``camelCase``|names – the name starts with a lowercase letter, <br>but new words start with an uppercase letter|	``firstName``|
|4|``PascalCaseNames``|(Also known as an upper camel) – similar to camelCase, <br>but the name starts with an uppercase letter, as do all additional words|	``FirstName``|
<br> 

### name-separator practices
This is only a quick reference to possible name-separator practices:<br>

|#|	Original data|	Description| Example |
| --- | --- | --- | --- |
|1| ``snake_case``|Using Snake Case, the empty spaces are replaced with underscores.|``first_name``|
|2| ``kebab-case``|Using Kebab Case, the empty spaces are replaced with dashes.|``first-name``|
<br>
### Policies
These standards are proposed. They apply to table names, but most also to attributes (column names).

#### Table Name – singular Noun
Use singular nouns for database table names.  <br>
Table names should be NOUNS. Nouns are the most descriptive way of representing an object.   <br>
For example: ``Customer``  <br>
Recommendation:    <br>
Do not use plural nouns, e.g., “Customers”.   <br>
<br><br>

#### Table Name – Use of Qualifier (adjective + Noun)
Think twice when using adjectives in conjunction with the nominated noun.    <br>
This could be a design flaw. Some adjectives are only reinforcements of data that is saved or known otherwise.   <br>
For example:    <br>
. ``singleCustomer``.    <br>
<br>
Recommendation:    <br>
Adjectives in the Table names are not recommended.    <br>
These adjectives can be modelled as attributes.   <br>
<br>
There are also legitimate business scenarios where there is a need to have qualifiers for business entities:   <br>
For example:    <br>
. ``Order``
. ``ReturnedOrder``
<br>
This can be modelled in multiple ways, but there is indeed the need to model “returned orders” to be distinguished in some way from the normal “orders”.    <br>
This can be by having a ``Boolean`` field in the Order table, or having a separate table for Orders that have been returned, etc. The problem with qualifiers is that in the future, there could be more qualifiers to the same object, for example:   <br>
. ``ShippedOrders``    <br>
. ``ApprovedOrders``   <br>
. ``CappedOrders``   <br>
. ``DraftOrders``   <br>
. Etc.   <br>
<br>
So, these qualifiers can be just simple attributes of the entity and status fields (Boolean) or a Category Table.    <br>
<br><br>
#### Table Name – Composed Names (Two Nouns)
This refers to when a table name is the composition of two nouns. This could be a valid scenario, but it could also be a design flaw.
Whenever you find this, please think carefully if these two are not separate entities that should have their table separate.
For example: to mention some:  <br>
. ``CustomerApplication`` <br>
. ``UserProfile`` <br>
 <br>
When they should be modelled as: ``Customer`` table  and ``Application`` table. <br>
And the ``User`` table and ``Profile`` table.  
Because it could be the case that ``Customer`` has N ``Application``<br>
Because it could be the case that ``User`` has N ``Profile``<br>
Recommendation: <br>
If you find a table name with two nouns, verify if these entities may be better in different tables. <br>
<br><br>
#### Prefix and Suffix 
Avoid using unnecessary prefixes or suffixes for table names and fields. <br>
For example:  <br>
. ``CustomerTable`` <br>
. ``TCustomer`` <br>
. ``tbl_customer`` <br>
. ``customer_column`` <br>
. ``customer_col`` <br>
. ``c_customer`` <br>
. Etc… <br>
<br> <br> 

#### Boolean fields prefix
To have consistency in naming and have a mnemotechnics rule to identify this type of field, the Boolean fields should start with the following prefixes: <br>
. ``is`` <br>
. ``has`` <br>
<br>
For example: <br>
In the ``Customer`` table, a flag to indicate is a "frequent" customer, and then the boolean flag field should be:<br>
. ``isFrequentFlyer``<br>
. ``hasCredential``<br>
<br>
Recommendation: 
Boolean fields are better understood if they have a prefix.
<br> <br> 
#### camelCase or PascalCaseName
This is part of the importance of being consistent. It is far more important than what naming style you use. So, we need to nominate a standard, adopt it and be consistent. <br>
Decide to adopt or not adopt the ``camelCase``.  <br>
For example: 
. ``customerProfile`` 
. ``localTime``
<br>
Adopt or do not adopt the ``PascalCaseName``. <br>
For example:  <br>
. ``CustomerProfile``  <br>
. ``LocalTime``  <br>
<br><br>
 #### Name separators 
This is part of the importance of being consistent. It is far more important than what naming style you use. So, we need to nominate a standard, adopt it and be consistent.<br>
Adopt or do not adopt the underscore (“_”) or dashes (“-“) as a name separator. <br>
For example: <br>
. ``user_profile``. <br>
. ``local_time``<br>
Or<br>
. ``user-profile`` <br>
. ``local-time``<br>
<br><br>

#### Special Characters
Don’t use special characters for table names or field names. Including Spaces, ``&``, ``*``, ``$``, ``@``, etc. Only alphanumeric characters. <br>
Only underscore (``_``), or dash (``-``) if the conventions adopted permits it.<br>
The database or the application may have problems with these. Moreover, some database engines do not accept it and replace it automatically. This can cause strange characters to appear as part of the name replacing the space.<br>
<br><br>

#### SQL Object types Prefix
Object types can adopt the following prefix conventions: <br>
. ``PK`` for primary key constraints.<br>
. ``FK`` for foreign key constraints.<br>
. ``UK`` for unique key constraints.<br>
. ``TG`` for triggers.<br>
. ``SP`` for stored procedures<br>
. ``IX`` for indexes.<br>
An index name should include the index type, the name of the indexed table, and indexed column names. It would look something like this:<br>
``IX_TableName_ColumnName``<br>
<br><br>
#### Use database features consistently
Consider using database properties for helping with data integrity<br>
For example:<br>
. Use Constraints, foreign key, check, not null ...etc to ensure data integrity. <br>
. Avoid giving the responsibility of SQL referential integrity to the Application<br>
<br>
Recommendation: <br>
Avoid implementing data integrity rules at the code-base level. Rely on database features for this.<br>
<br><br>
#### Consistency in standards adoption (Consistency)
It is encouraged to align with what other development groups have adopted already.<br>
For example, align with other domain names and data types if they have modelled this entity.<br>
All microservices will be part of one system. <br>
When you're thinking about a table name, please take the time to explore how the same entity has been called somewhere else. <br>
If they differ, you must discuss and agree with other domains of these conventions. <br>
If you are right, then tag the other domain deviation on the naming convention as technical debt and carry on.<br>
If the name used on the other domain is right, then create a registry of decisions to capture this important insight and carry on.<br>
<br>
## References
<br> <br> 
<br> 
# SQL Reserved words
## Category
## Description
Avoid using reserved words when naming databases, tables, columns, or objects. <br>
The following table contains a list of the common reserved words:<br>
``ABORT``, ``ALL``, ``ANALYSE``, ``ANALYZE``, ``AND``, ``ANY``, ``ASC``, ``BETWEEN``, ``BINARY``, ``BIT``, ``BOTH``, ``CASE``, ``CAST``, ``CHAR``, ``CHARACTER``, ``CHECK``, ``CLUSTER``, ``COALESCE``, ``COLLATE``, ``COLLATION``, ``COLUMN``, ``CONSTRAINT``, ``COPY``, ``CROSS``, ``CURRENT``, ``CURRENT_CATALOG``, ``CURRENT_DATE``, ``CURRENT_DB``, ``CURRENT_SCHEMA``, ``CURRENT_SID``, ``CURRENT_TIME``, ``CURRENT_TIMESTAMP``, ``CURRENT_USER``, ``CURRENT_USERID``, ``CURRENT_USEROID``, ``DEALLOCATE``, ``DEC``, ``DECIMAL``, ``DEFAULT``, ``DECODE``, ``DESC``, ``DISTINCT``, ``DISTRIBUTE``, ``DO``, ``ELSE``, ``END``, ``EXCEPT``, ``EXCLUDE``, ``EXPLAIN``, ``EXTEND``, ``EXTERNAL``, ``EXTRACT``, ``FALSE``, ``FIRST``, ``FLOAT``, ``FOLLOWING``, ``FOR``, ``FOREIGN``, ``FROM``, ``FULL``, ``FUNCTION``, ``GENSTATS``, ``GLOBAL``, ``GROUP``, ``HAVING``, ``IDENTIFIER_CASE``, ``ILIKE``, ``IMPORT``, ``IN``, ``INDEX``, ``INITIALLY``, ``INNER``, ``INOUT``, ``INTERVAL``, ``INTO``, ``IS``, ``JOIN``, ``KEY``, ``LAST``, ``LEADING``, ``LEFT``, ``LIKE``, ``LIMIT``, ``LOAD``, ``LOCAL``, ``LOCK``, ``MINUS``, ``MOVE``, ``NATURAL``, ``NCHAR``, ``NEW``, ``NOT``, ``NOTNULL``, ``NULL``, ``NULLS``, ``NVL``, ``NVL2``, ``OFFSET``, ``OLD``, ``ON``, ``ONLINE``, ``ONLY``, ``OPEN``, ``OR``, ``ORDER``, ``OTHERS``, ``OUTER``, ``OVER``, ``OVERLAPS``, ``PARTITION``, ``POSITION``, ``PRECEDING``, ``PRECISION``, ``PRIMARY``, ``RESET``, ``REUSE``, ``RIGHT``, ``ROWS``, ``SELECT``, ``SETOF``, ``SHOW``, ``SOME``, ``TABLE``, ``THEN``, ``TIES``, ``TIME``, ``TIMESTAMP``, ``TO``, ``TRAILING``, ``TRANSACTION``, ``TRIGGER``, ``TRIM``, ``TRUE``, ``TRUNCATE``, ``UNBOUNDED``, ``UNION``, ``UNIQUE``, ``USER``, ``USING``, ``VACUUM``, ``VERBOSE``, ``VERSION``, ``VIEW``, ``VIRTUAL``, ``WHEN``, ``WHERE``, ``WITH``, ``WRITE``.
## References
<br> <br> 
<br> 
# Source of Truth
## Category
## Description
This design principle applies especially when designing distributed systems.  <br> 
Therefore it is important to consider having only one master always.  <br> 
This is applicable when designing microservices and Domain Driven Design (DDD) systems.  <br> 
In microservices architecture is important to nominate which microservice is mastering business entities. <br> 
Only one domain should master a business entity. <br> 
However, some entities can be in multiple domains/microservice.  <br> 
If this is the case, it is expected to be mastered in one domain/microservice and replicated in others through eventual consistency. <br> 
Avoid and challenge if you find a business entity is mastered in more than one domain/microservice. <br> 
 <br> 
For example:
In the logical domain model, the entities identified as mastered in this data model are coloured Gold. <br> 
<br><img src="./images/Source-of-truth1.jpg" align="center" width=80% height=80%> <br> <br> 
## References
<br> <br> 
<br> 

# Party (data modelling standardization)
## Category
## Description
Cataloguing Parties is quite a big effort to have consistency across the whole company. <br> 
Therefore, consultation with data modellers about the correct party name that should be used in your domain would be required. <br> 
Parties refer, for example, to:
***Organization***<br>
. ClientCompany <br> 
. Tenant <br> 
. Vendor <br> 
. AfiliatedCompany <br> 
. Supplier <br> 
. Etc <br> 
 <br> 
***Individual*** <br> 
. User <br> 
. ClientIndividual <br> 
. Customer <br> 
. etc. <br> 
 <br> 
***Other*** <br>
. Application (Third party application interacting with Pickles) <br> 
. Etc.  <br> 
#### Party and PartyRole diagram
<br><img src="./images/Party-PartyRole-diagram1.jpg" align="center" width=100% height=100%> <br> <br>
## References
<br> <br> 
<br> 
# PartyRoles Relationships (modelling)
## Category
## Description
Following the “Party” diagram above. <br> 
The party are abstract entities that take a role in a scenario. They use an envelope of PartyRole when the Party is used in an Application-Channel. <br> 
PartyRoles are related through “PartyRoleAssociation”.  <br> 
This PartyRoleAssociation enables a direct relationship between Parties.  <br> 
However, this is not to be taken literally since a relationship between parties should be more dynamic rather than static.  <br> 
From the modelling point of view, the relationship or association between two parties, in general, must be through a: <br> 
. An “Agreement” <br> 
. A “Service”  <br> 
. A “Contract” <br> 
So that the two parties have a common ground of understanding and other meta-data that put context to this association or understanding between parties. <br> 
## References
<br> <br> 
<br> 
# First Name – Last Name
## Category
## Description
There are several considerations for designing First Name, Last Name, Middle Name, etc.  <br> 
Firstly, avoid storing a person’s name in a single field. <br> 
Secondly, consider all possible applicable conditions to the names for the culture the application is being designed. <br> 
It is important to design the Individual’s name carefully from the beginning. <br> 
There are multiple ways these can be used. See references. <br> 
## References
https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/
<br> <br> <br> 
--End of the File--
