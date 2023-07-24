# db-schema-design-guidelines
Database SQL schema design and data modelling guidelines

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./images/db-schema-design-guidelines.jpg">
  <source media="(prefers-color-scheme: light)" srcset="./images/db-schema-design-guidelines.jpg">
  <img alt="Shows an illustrated sun in light mode and a moon with stars in dark mode." src="./images/db-schema-design-guidelines.jpg">
</picture>
<br>
<br>
## Intro
### Summary
This document lists different patterns for logical and physical data modelling. <br>
The list is not the full list of guidelines but a collection of best practices to be considered when producing logical, physical data models and avoiding re-work or technical debt. 
<br>
<br>
**Architecture Governance** <br>
Architects can benefit from database SQL schema and data modelling guidance from several viewpoints. <br>
Standardization of data modelling: Guidelines and standard SQL schema designs ensure consistency and standardization across projects, fostering a unified data architecture and facilitating collaboration. <br>
Improved Data Quality: Following guidelines leads to data normalization, validation, and integrity, improving data quality, reducing inconsistencies, and enhancing data reliability.
Scalability: Best practices in data modeling cater to scalability, allowing data structures to accommodate growth and business changes without major restructuring or performance issues. <br>
Efficient Data Retrieval: Logical and physical data modeling patterns optimize data storage and retrieval, leveraging techniques like indexing and columnar storage to boost system performance.  <br>
Easier Maintenance: Well-designed data models ease database schema maintenance, minimizing schema changes and avoiding technical debt.  <br>
Enhanced Security: Data modeling guidelines incorporate security measures, ensuring the protection of sensitive data, compliance with privacy regulations, and safeguarding organizational data.  <br>
Alignment with Business Goals: Architects align the data model with business goals and requirements, crucial for delivering valuable insights and supporting decision-making processes.  
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
For example: <br>
## Resources
**SQL Fluff	Source code quality	SQL linter** <br>
Source code linter specialising in SQL statements.	SQL Linter <br>
https://docs.sqlfluff.com/en/stable/ <br> 
<br>
**SQL Lint	Source code quality	SQL Linter**<br>
Source code linter specialising in SQL statements.	SQL Linter  <br>
https://github.com/joereynolds/sql-lint
<br>
<br>
<br>
--End of File--
