# Big Data Orientation
## Lab 2 – Working with a Relational Database in Microsoft Azure
### Overview
1. Azure SQL Database is a cloud service based on the Microsoft SQL Server relational database management system (RDBMS).
2. Application developers can use Azure SQL Database as a relational store for application data, which can be used in big data solutions.
3. In addition to Azure SQL Database, Azure includes a data warehouse service named **Azure SQL Data Warehouse**, which shares the same core database engine as Azure SQL Database but is optimized for large data workloads, and often provides an analytical data store into which big data processing solutions load the processed data for analysis and reporting.

### In this lab, we will provision and work with Azure SQL Database. The tasks we will perform in this exercise can also be performed with Azure SQL Data Warehouse.
#### To complete this lab, you will need the following:
-  A web browser
-  A Windows, Linux, or Mac OS X computer

### Exercise 1: Working with Azure SQL Database
In this exercise, you will provision a sample database in Azure SQL database, and use Transact-SQL to query the data it contains.
### Step-1
1. In the Microsoft Azure portal, in the menu, click New. Then in the Databases menu, click SQL Database.
2. In the SQL Database blade, enter the following settings, and then click Create:
-  Name: **AdventureWorksLT**
- Subscription: Select your Azure subscription
- Resource Group: Select the resource group you created previously
- Select source: **Sample** (AdventureWorksLT)
- Server: Create a new server with the following settings:
- Server name: Enter a unique name (and make a note of it!)
- Server admin login: Enter a user name of your choice (and make a note of it!)
- Password: Enter and confirm a strong password (and make a note of it!)
- Location: Select any available region
- Allow azure services to access server: Selected
In the Azure portal, view Notifications to verify that deployment has started. Then wait for the SQL database to be deployed (this can take a few minutes.)

## Query a Table
- A relational database contains tables, each of which contains data. 
- Tables are organized into namespaces called schemas – in the case of the **AdventureWorksLT** sample database, most of the tables are defined within a schema named **SalesLT**.

## Step-2
1. Click All Resources, and then click the AdventureWorksLT database.
2. On the AdventureWorksLT blade,In the toolbar for the **query editor**, click Login, and then log into your database using SQL Server authentication and entering the login name and password you specified when provisioning the Azure SQL Database server.
3. In the query editor, enter the following Transact-SQL query to retrieve the contents of the SalesLT.Product table in the AdventureWorksLT database:
```
SELECT * FROM SalesLT.Product;
```
## Exercise 2: Loading Data into a Database
In this exercise, you will create a table in the sample database you created previously, and then use Azure Data Factory to copy data from a file in Azure Storage into the new table.
### Create Table
The sample database contains many tables, and you can add your own by using the Transact-SQL CREATE TABLE statement.
- In the Query pane, replace the existing SELECT statement with the following code:
```
CREATE TABLE SalesLT.ProductReview
( ProductReviewID INTEGER PRIMARY KEY,
ProductID INTEGER REFERENCES SalesLT.Product(ProductID),
ReviewerName NVARCHAR(25),
ReviewDate DATETIME,
EmailAddress NVARCHAR(50),
Rating INTEGER,
Comments NTEXT );
```

