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



### NORMALIZATION AND DENORMALIZATION
#### NORMALIZATION
Normalization is used to remove redundant data from the database and to store non-redundant and consistent data into it (data integrity)

##### Why Normalization
- Normalization maintains data integrity i.e. any addition or deletion of data from the table will not create any mismatch in the relationship of the tables.
- 	Normalization is generally used where number of insert/update/delete operations are performed and joins of those tables are not expensive.

### Types of Normalization
The first three forms of database normalization are

- First Normal Form (1 NF)
- Second Normal Form (2 NF)
- Third Normal Form (3 NF)

*Examples*
#### 1NF conditions
- Different relation to different table
- Each cell contains unique values , you can add data values without overwriting any present data.

Look at this table, you see the music table has more than one value in music genre and artist_name
and it disobeys 1NF 
| Music_Id    | Music_Name | Genre  |Artist_Name |
| ----------- | ----------- | ----------- |-----------  |
| 1     | Super Bass      |Rap,Pop       |Nicki Minaj       |
| 2    | Halo       |R & B        |Beyonce      |
| 2    | Feeling Myself       |Rap       |Nicki Minaj,Beyonce |

Let us Normalize it now.
The final table will look like below

| Genre_ID      | Genre_Name |           
| ----------- | ----------- |
| 1      | Pop       |
| 2   |  Rap        |
| 3   |  R & B       |


| Artist_ID     | Artist_Name |
| ----------- | ----------- |
| 1      | Nicki Minaj       |
| 2   | Beyonce        |


| Music_Id    | Music_Name | Genre_ID  |Artist_ID |
| ----------- | ----------- | ----------- |-----------  |
| 1     | Super Bass      |1       |1       |
| 1     | Super Bass      |2       |1       |
| 2    | Halo       |3       |2     |
| 2    | Feeling Myself       |2       |1 |
| 2    | Feeling Myself       |2       |2|

#### 2NF conditions
A table is said to be in 2NF if both the following conditions hold:

- Table is in 1NF (First normal form)
- In the second normal form, all non-key attributes are fully functional dependent on the primary key

An E-learning platform , the tables for content creators
It is not in 2NF because non prime attribute creator date of birth is dependent on creator_id alone . This violates the rule for 2NF as the rule says all non-key attributes are fully functional dependent on the primary key

| Content_Id    | Content_Name | Creator_ID  |Date_of_birth |
| ----------- | ----------- | ----------- |-----------  |
| 1     | Photography      |3      |12/12/1890       |
| 2    | Visual Arts       |4       |12/02/1888      |


After Normalization, we end up with the following

| Content_Id    | Content_Name | Creator_ID  |
| ----------- | ----------- | ----------- |
| 1     | Photography      |3      |
| 2    | Visual Arts       |4       |


| Creator_ID  |Date_of_birth |
| ----------- |-----------  |
|3      |12/12/1890       |
|4       |12/02/1888      |


####  3NF conditions
A table is said to be in 3NF if both the following conditions hold:

- A Table is already in 2 NF
- Non-Primary key columns shouldn’t depend on the other non-Primary key columns
- There is no transitive functional dependency

 The Music_Id determines the Album_ID of an Artist, instead of the album_id that determines the album name. Therefore, the album name column is indirectly dependent on the Music column. So, it satisfies the transitive dependency. So this cannot be in third normal form.

| Music_Id    | Music_Name | Genre  |Artist_Name |Album_Name |
| ----------- | ----------- | ----------- |-----------  |-----------  |
| 1     | Super Bass      |Rap      |1       | Pink Friday |
| 2    | Barbie Dreams      |Rap      |1       | Queen|

After Normalization

| Music_Id    | Music_Name | Genre  |Artist_Name |Album_ID |
| ----------- | ----------- | ----------- |-----------  |-----------  |
| 1     | Super Bass      |Rap      |1       | 1 |
| 2    | Barbie Dreams      |Rap      |1       | 2|


| Album_ID     | Album_Name |
| ----------- | ----------- |
| 1      | Pink Friday     |
| 2   | Queen        |


#### DENORMALIZATION
###### Why Denormalization
- Denormalization is used to combine multiple table data into one so that it can be queried quickly.
- Denormalization on the other hand focus on to achieve the faster execution of the queries through introducing redundancy.
- Denormalization is used where joins are expensive and frequent query is executed on the tables.
- Denormalization does not maintain any data integrity.


### References
- [Data Modelling](https://www.ibm.com/cloud/learn/data-modeling)
- [OLTP vs OLAP](https://www.stitchdata.com/resources/oltp-vs-olap/)
- [Normalization and Denormalization](https://www.tutorialspoint.com/difference-between-normalization-and-denormalization) 
- [INF,2NF,3NF] (https://www.sqlshack.com/what-is-database-normalization-in-sql-server/)