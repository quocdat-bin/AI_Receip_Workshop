---
title : "Creating and Configuring Amazon RDS MySQL"
date : 2026-07-07 
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---

### 1. Create Amazon RDS for MySQL
Log in to the AWS Management Console.
- Go to the Amazon RDS service.
![endpoint diagram](/images/5-Workshop/5.3-Backend/3.jpg)
- Choose Create database in Amazon RDS.
![endpoint diagram](/images/5-Workshop/5.3-Backend/4.jpg)

-	Database engine: MySQL.
-	Create method: Full configuration
-	Template: Dev/Test 
![endpoint diagram](/images/5-Workshop/5.3-Backend/5.jpg)

Availability and durability: Single-AZ DB instance deployment (1 instance)
![endpoint diagram](/images/5-Workshop/5.3-Backend/6.jpg)

- Name the DB instance.
- Declare the database administrator account.
- Set a strong password for the administrator account.

![endpoint diagram](/images/5-Workshop/5.3-Backend/7.jpg)

- Choose the DB instance class db.t3.micro.
- Configure 20 GiB of storage.
- Disable or limit storage autoscaling if it is not needed.
- Choose an appropriate VPC and subnet group.
- Configure public access during the development phase if you need to connect from a personal machine.
- Choose or create a Security Group for RDS.
- Keep the default MySQL port 3306.

![endpoint diagram](/images/5-Workshop/5.3-Backend/8.jpg)

- Review the entire configuration.
- Choose Create database.
- Wait for the DB instance to change to the Available state.

![endpoint diagram](/images/5-Workshop/5.3-Backend/9.jpg)

### 2. Configure the Security Group

- Open the DB instance's connection information.
- Access the Security Group attached to RDS.
- Open the Inbound rules section.
- Add a rule for the MySQL/Aurora service.
- Choose the TCP protocol and port 3306.
- Restrict the access source to the development machine's public IP address.
- Save the Security Group configuration.
- Update the IP address in the rule if your current network changes.
- Do not keep 0.0.0.0/0 access in the production environment.
- After deploying the backend, restrict RDS connections to the EC2 Security Group.

![endpoint diagram](/images/5-Workshop/5.3-Backend/10.jpg)

### 3. Prepare the SSL Connection
- Download the CA bundle certificate for Amazon RDS.
- Save the file as global-bundle.pem.
- Place the certificate in a secure local folder.
- Do not upload the private certificate, passwords, or .env files to GitHub.
- Configure the MySQL Client or MySQL Workbench to use SSL.
- Choose the server identity verification mode when connecting.
- Verify the certificate can be read successfully by the program.
### 4. Connect with MySQL Workbench
- Install MySQL Workbench on the development machine.
- Open MySQL Workbench.
- Create a new MySQL connection.
- Enter the Amazon RDS endpoint in the hostname field.
- Enter port 3306.
- Enter the RDS administrator account.
- Configure the global-bundle.pem file in the SSL section.

![endpoint diagram](/images/5-Workshop/5.3-Backend/11.jpg)

- Choose Test Connection.
- Enter the administrator password when prompted.

![endpoint diagram](/images/5-Workshop/5.3-Backend/12.jpg)

- Verify the connection succeeds.
- Save the connection configuration for use in the following steps.

### 5. Create the Schema Used by the System
**Check the list of existing databases:**
- SHOW DATABASES;
- Create the project's main schema:
- CREATE DATABASE food_ai_db
- CHARACTER SET utf8mb4
- COLLATE utf8mb4_unicode_ci;
- Select the newly created schema:
- USE food_ai_db;
- Check the schema currently in use:
- SELECT DATABASE();

### 6. Create the Tables on RDS
- Open the prepared schema.sql file.
- Select the food_ai_db schema.
- Run the table-creation statements.
- Create the tables one by one:
    - users
    - recipes
    - nutrition
    - favorites
    - search_history
    - chat_history
- Check the list of tables:
    - SHOW TABLES;
- Check the structure of each table:
    - DESCRIBE users;
    - DESCRIBE recipes;
    - DESCRIBE nutrition;
    - DESCRIBE favorites;
    - DESCRIBE search_history;
    - DESCRIBE chat_history;
- Check the primary keys, foreign keys, and data constraints.
- Confirm the JSON fields are compatible with MySQL.
- Confirm the charset of the tables is utf8mb4.

### 7. Create Indexes

- Open the indexes.sql file.
- Select the food_ai_db schema.
- Run the index-creation statements.
- Create an index for searching recipe names.
- Create an index to support querying the favorites list.
- Create a composite index for search history by user and time.
- Create a composite index for chat history by user and time.
- Create an index for the recipe referenced in the chat history.
- Check the indexes of each table:
    - SHOW INDEX FROM recipes;
    - SHOW INDEX FROM favorites;
    - SHOW INDEX FROM search_history;
    - SHOW INDEX FROM chat_history;
- Confirm there are no redundant or duplicate indexes.

### 8. Configure the Connection for the Program

- Create a .env file in the backend runtime environment or the import script.
- Declare the necessary connection variables, for example:
    - DB_HOST=<rds-endpoint>
    - DB_PORT=3306
    - DB_NAME=food_ai_db
    - DB_USER=<database-user>
    - DB_PASSWORD=<database-password>
- Configure the program to read the environment variables.
- Do not hard-code the endpoint, username, or password in the code.
- Add .env to .gitignore.
- Create .env.example containing only the variable names and sample values.
- Test the connection from Python to food_ai_db.
- Confirm the program can open and close the database session normally.

### 9. Import Data into Amazon RDS MySQL
#### 9.1. Prepare the Import Environment

- Check the endpoint, port 3306, and the food_ai_db schema.
- Check that the Security Group allows the import machine to connect to RDS.
- Test the connection with MySQL Workbench.
- Place the two processed data files in the seed folder:
    -    backend/database/seed/
    - 	 recipes_cleaned.pkl
    - 	 nutrition_cleaned.pkl
- Place the import script at:
    - backend/database/scripts/import_data.py
- Create and activate a Python virtual environment.
- Install the necessary libraries:
    - pip install pandas sqlalchemy pymysql python-dotenv
- Re-check the data to be imported

#### 9.2. Read the Processed Data

- Read recipes_cleaned.pkl and nutrition_cleaned.pkl with Pandas.
- Check the number of records, number of columns, column names, data types, missing values in required fields, and duplicate primary keys.

#### 9.3. Prepare the Nutrition Data
- Select the fields to import:
- Compare the length of food_name and data_type with the table structure.
- Confirm the nutrition data is ready to import.

#### 9.4. Import the Nutrition Data

- Split the data into small batches.
- Open the connection to Amazon RDS.
- Insert the data into the nutrition table.
- Close the connection after completion.
- Check the total number of records and a random sample of records

#### 9.5. Import the Recipe Data

- Split the recipe data into multiple batches.
- Do not send all of the more than 400,000 records in a single transaction.
- Open the connection to Amazon RDS.
- Insert each batch into the recipes table.
- Close the connection after completion.

#### 9.6. Check the Quantity and Quality of the Data on RDS

- Count the total number of recipes:
- Count the total amount of nutrition data:
- Compare the results with the import report.
- Check the number of skipped records.
- Confirm no duplicate primary-key records were created.
- Randomly check the recipe data.
- Cross-check the ingredients, cooking steps, tags, and the count fields.

#### 9.7. Check Indexes and Query Performance

- Check the indexes on the recipes table:
- Run a test query to search for dish names:
    - SELECT recipe_id, name
    - FROM recipes
    - WHERE name LIKE 'chicken%'
    - LIMIT 20;
- Check the query that retrieves recipe details by primary key.
- Check the query that retrieves nutrition by fdc_id.
