### DATA MODELLING 

Data modeling is the process of creating a visual representation of either a whole information system or parts of it to communicate connections between data points and structures.


#### WHY RELATIONAL DATABASE
- Flexibility for writing in SQL queries: With SQL being the most common database query language.
- Modeling the data not modeling queries
- Ability to do JOINS
- Ability to do aggregations and analytics
- Secondary Indexes available : You have the advantage of being able to add another index to help with quick searching.
- ACID Transactions: Allows you to meet a set of properties of database transactions intended to guarantee validity even in the event of errors, power failures, and thus maintain data integrity.
A-Atomicity - The whole  query transaction is processed or nothing is processed.
C-Consistency - e.g. If a column is defined as boolean, a string or numerical table cannot be committed into the column, it would raise an SQL IntegrityError.
I-Isolation - e.g multiple query transactions can occur without interfering with any other.
D-Durability - e.g  DB sessions that has been committed will remain, even in case of power failure


#### WHEN NOT TO USE RELATIONAL DB
- Have large amounts of data: Relational Databases are not distributed databases and because of this they can only scale vertically by adding more storage in the machine itself. You are limited by how much you can scale and how much data you can store on one machine. You cannot add more machines like you can in NoSQL databases.
- Need to be able to store different data type formats: Relational databases are not designed to handle unstructured data e.g  A column can take strings, numerical values or blobs etc.
- Need high throughput -- fast reads: While ACID transactions bring benefits, they also slow down the process of reading and writing data. If you need very fast reads and writes, using a relational database may not suit your needs.

#### WHEN NOT TO USE NOSQL DB

- When you need the ability to do JOINS across tables: NoSQL does not allow the ability to do JOINS. This is not allowed as this will result in full table scans.
- If you want to be able to do aggregations and analytics
- If you have changing business requirements : Ad-hoc queries are possible but difficult as the data model was done to fix particular queries




#### OLTP and OLAP
OLTP and OLAP: The two terms look similar but refer to different kinds of systems. 
- Online transaction processing (OLTP) captures, stores, and processes data from transactions in real time. 
OLTP is based on the following
    - Based on INSERT, UPDATE, DELETE commands
    - Normalized databases for efficiency
    - Short, fast updates initiated by user

- Online analytical processing (OLAP) uses complex queries to analyze aggregated historical data from OLTP systems.
    - Based on SELECT commands to aggregate data for reporting
    - Lost data can be reloaded from OLTP database as needed in lieu of regular backups
    - Denormalized databases for analysis
    - Data periodically refreshed with scheduled, long-running batch jobs

The data from one or more OLTP databases is ingested into OLAP systems through a process called extract, transform, load (ETL).