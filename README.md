Datastax Astra DB is a fully managed, multi-cloud and serverless DBaaS that scales up and down dynamically with pay-as-you-go pricing.  

Astra DB eliminates operational overhead.  Day 2 operations such as backup and patch will be managed by Datastax.  

It is serverless so it avoids overprovisioning and reduces TCO by matching resources to application requirements and data traffic.  Customers deploy richly interactive modern data apps that can scale infinitely from day 1 and scale down to zero automatically when the database is not in use.

It is available on any of the major public clouds (AWS, GCP, Azure) in one or more regions so there is no vendor lock-in. 

Datastax offers free credit of $25/month for trial use.   You will be using this free credit account for this workshop.   With just a few clicks,  you will register to AstraDB.   No credit card information is required. 

## Section A: Register to Astra DB

1. Open a new browser and access http://astra.datastax.com.  Click on **[Sign In With Google]** to sign up with a gmail account or click on the **Sign Up** link below to sign up with your work email address. 
![image](img/image1.png?raw=true)

2. For this workshop,  I will use a personal gmail account to sign up.  Click on **[Sign In With Google]**.

3. Specify **gmail email address**.   Click on [**Next**].
![image](img/image2.png?raw=true)

4. Specify the **password**.  Click on [**Next**].
![image](img/image3.png?raw=true)

5. You are registered to Astra DB
![image](img/image4.png?raw=true)

6. From the dashboard, you can see your plan, credit usage / amount billable (will see that after creating your database, your usage.

## Section B: Create Database 

1. Click on [**Create Serverless Database**].

2. Specify the **Database Name** as **shinydb** and **Keyspace Name** as **shiny**.   **Keyspace Name** is like Schema Name in Oracle, MySQL etc.   Choose **Google Cloud** as the **cloud provider** and “**Moncks Corner, South Carolina  (us-east1)**” as the **region**.  Click on [**Create Database**].   You are free to use other names too.
![image](img/image5.png?raw=true) 

3. This will take a few minutes.
![image](img/image6.png?raw=true)

4. Click on **Download Token Details**.   In the subsequent exercises,  you need this token to access the database from your application.    Please note that you cannot retrieve the token later. 

5. Click on **go to the dashboard** link.   Please note that credit usage is shown now.   If status is not shown as Active yet,  please wait for a few more minutes.  If status is Active,  please click on the newly created **database** (e.g. **shinydb**). 
![image](img/image7.png?raw=true)

6. You can monitor the usage of this database under the **Overview** tab.   You can add more regions to this database.  You can also add more keyspace.
![image](img/image8.png?raw=true)

7. Click on the **Health** tab.  You can view health metrics that include information regarding latency and throughput to the database.
![image](img/image9.png?raw=true)

8. Click on the **Connect** tab.   There are various ways to connect Astra DB to access your data.
![image](img/image10.png?raw=true)

9. Click on the **CQL Console** tab.  The Cassandra Query Language SHell (CQLSH) is a command line shell for interacting with your database through Cassandra Query Language (CQL). This tool provides a useful interface for accessing the database and issuing CQL commands.   You will be using it later in this exercise. 
![image](img/image11.png?raw=true)

10. Click on the **CDC** tab.   This tab is enabled only if the database is created in a region where AstraStreaming is available.   This is to enable change data capture for the tables in this database.  If this CDC is enabled,  any insert / update / delete will be captured and data will be published to Astra streaming in the same region as this database. 
![image](img/image12.png?raw=true)

11. Click on the **Settings** tab.    You can restrict which public endpoints are able to access your database,  connect to a private endpoint.  You can terminate the database if it is not required. 
![image](img/image13.png?raw=true)

## Section C : CQL Interactive 

1. You can create the tables in CQL Console.   

2. Let's start with the CQL.  Click on the **CQL Console** tab.

3. Type “**use shiny;**”.  This is to use the **shiny** keyspace.  Please use the keyspace name that you set earlier.  
![image](img/image14.png?raw=true) 

4. Copy and paste the script below to create the **product_by_category** table.   This is a table to store products that are partitioned using category and uniquely identified using id and category. 
```copy
CREATE TABLE product_by_category  (
    category text,
    id uuid,
    name text,
    description text,
    quantity int,
    price double,
    PRIMARY KEY ((category),  id)
);
```
5. You can check if the table is created successfully by type **describe product_by_category**.
![image](img/image15.png?raw=true)
 
6. Next, copy and paste the script below to insert a record into the **product_by_category** table.
```copy
Insert into product_by_category (category, id, name, description, quantity, price) values('Foods', now(), 'XYZ Chicken Wings', 'XYZ Chicken Wings', 20, 15.99);
```

7. Next, copy and paste the script below to get the id from the 1st select query and replace <id> with the id from DB and update a record in the **product_by_category table**.
```copy
Select * from product_by_category; 

Update product_by_category set quantity = 18 where category = 'Foods' and id = <id>;
```

8. Next, copy and paste the script below to get the id from the 1st select query and replace <id> with the id from DB and delete the record in the **product_by_category** table.
```copy
Select * from product_by_category; 

Delete from product_by_category where category = 'Foods' and id = <id>;
```


 
