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
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/azure_sql1.png)
## Query a Table
- A relational database contains tables, each of which contains data. 
- Tables are organized into namespaces called schemas – in the case of the **AdventureWorksLT** sample database, most of the tables are defined within a schema named **SalesLT**.

## Step-2
1. Click All Resources, and then click the AdventureWorksLT database.
2. On the AdventureWorksLT blade,In the toolbar for the **query editor**, click Login, and then log into your database using SQL Server authentication and entering the login name and password you specified when provisioning the Azure SQL Database server.
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img2.png)
3. In the query editor, enter the following Transact-SQL query to retrieve the contents of the SalesLT.Product table in the AdventureWorksLT database:
```
SELECT * FROM SalesLT.Product;
```
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img_3.png)
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
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img4.png)
-  Replace the CREATE TABLE statement with the following SELECT statement:
```
SELECT * FROM SalesLT.ProductReview;
```
- Click Run, and verify that the query succeeds but returns 0 rows.
- Close the query editor without saving any changes.

## LOAD data from a file into the Azure
1. A common task in a big data solution is to transfer data from one store to another.
2. In this case, you will use the **Copy Data wizard** in the **Azure Data Factory** service to **load** the product review **data** from a **text file** in **Azure Storage** into the **table** you created in **Azure SQL Database**.
### Steps
1. In the Microsoft Azure portal, in the menu, click New. Then in the Data + Analytics menu, click Data Factory.
2. In the New data factory blade, enter the following settings, and then click Create:
- Name: Enter a unique name (and make a note of it!)
- Subscription: Select your Azure subscription
- Version: 1
- Resource Group: Select the resource group you created previously
- Location: Select any available region
- Pin to dashboard: Unselected
3. View Notifications to verify that deployment has started. Then wait for the data factory to be deployed (this can take a few minutes.)
4. Click All Resources, and then click your **data factory**, and click the **Author & Monitor** tile to launch the Data Factory UI application. This opens a new tab in your browser.
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img6.png)
5. Now click on the **Copy Data**.
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img7.png)
6. On the **Properties page** of the Copy Data wizard, enter the following details and then click Next:
- Task name: Load Reviews
- Task description: Load review data into Azure SQL Database
- Task cadence (or) Task schedule: Run once now
- Expiration time: 3:00:00:00
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img8.png)
7. On the Source data store page, on the Connect to a **Data Store tab**, select **Azure Blob Storage**. Then click Next.
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/img9.png)
8. On the Specify the **Azure Blob storage account** page, enter the following details and then click Next:
- Connection name: blob-store
- Account selection method: From Azure subscriptions
- Azure subscription: Select your subscription
- Storage account name: Select your storage account
9. On the Choose the input file or folder page, double-click the bigdata blob container you created previously and the data folder, and select the reviews.txt file. Then click Choose, and click Next.
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/imgn1.png)
10. On the File format settings page, wait a few seconds for the data to be read, and then verify the following details, ensuring that the rows of data in the Preview section match the table below, and click Next:
- File format: text format
- Column delimiter: Tab (\t)
- Row delimiter: Carriage return and line feed (\r\n)
- Skip line count: 0
- Column names in first data row: Selected
- Treat empty column value as null: Selected
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/imgn2.png)
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/imgn3.png)
10. On the Destination data store page, on the Connect to a Data Store tab, select Azure SQL Database. Then click Next.
11. On the Specify the Azure SQL database page, enter the following details and then click Next:
- Connection name: sql-database
- Server / database selection method: From Azure subscriptions
- Azure subscription: Select your subscription
- Server name: Select your Azure SQL server
- Database name: AdventureWorksLT
- User name: The server admin login name you specified when creating the database
- Password: The password for your Azure SQL server admin login
12. On the Table mapping page, in the Destination list, select [SalesLT].[ProductReview] and click Next.
13. On the Schema mapping page, ensure that the following settings are selected, and click Next:
![alt text](https://github.com/udayallu/BigData_Azure_sql_db/blob/master/Pics/imgn0.PNG)
14. On the Performance settings page, expand Advanced settings to review the default values. Then click Next.
15. On the Summary page, click Finish.
16. On the Deploying page, wait for the deployment to complete.

