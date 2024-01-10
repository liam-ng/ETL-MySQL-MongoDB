# ETL-MySQL-MongoDB
Attempt to migrate Hierarchical data to NoSQL which involves: 
(1) Queries to extract data from Azure Database for MySQL to storage blob, 
(2) Azure Data Factory Pipelines to transform SQL into document, 
(3) JSON data to be loaded into MongoDB.

## High Level Diagram
![image](https://github.com/liam-ng/ETL-MySQL-MongoDB/assets/90180576/36b7b2de-5ce4-4fbe-b50c-e1763344cf41)

## Configuration Procedures 
1. [[Create a MySQL]]
2. Import [Northwind.sql](https://github.com/harryho/db-samples/blob/master/mysql/northwind.sql)
	![[capture-20240107-2022 1.png]]
  Credit: harryho - Northwind sample database for MySql
3. Configure Azure Data Factory (Library included in this repo)

##Source Data Structure - ER Diagram
![image](https://github.com/liam-ng/ETL-MySQL-MongoDB/assets/90180576/dcacba7f-2340-46f0-840e-35f90d58be41)
Only Primary Keys and Foreign Keys are shown.

##Planned Schema Design
![image](https://github.com/liam-ng/ETL-MySQL-MongoDB/assets/90180576/1ad13aed-8417-41ca-a46c-2684ab8feef6)

**Good**:
- Excellent Performance (4ms)

**Bad**:
- Data duplication
- Large update effort

There are 3 approaches to this problem:
1. Embed (single document)
2. Reference (separate collections)
3. **Extended Reference** (separate collections but with duplicated data)

**To be Fixed**:
**Balance of storage cost and performance**
- Separate `product`, `customer`, `shipper` from `orderdetail`
- Keep frequently access information in `orderdetail` document, such as, `productName`, `companyName`, `contactName`  
- Reference new collections with `$lookup` operations
