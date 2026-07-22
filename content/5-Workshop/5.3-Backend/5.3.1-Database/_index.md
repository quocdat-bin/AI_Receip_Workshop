---
title : "Preparing the Database"
date : 2026-07-07 
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

### Step 1: Preparation 
- Download the Dataset 
![endpoint diagram](/images/5-Workshop/5.3-Backend/1.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/2.jpg)

### Step 2: DATA PROCESSING

Compile statistics on the number of records and data fields in Food.
- Dataset-processing/data/reports/recipes/profile.json

Convert the ingredient, cooking-step, and dish-tag fields into the correct structure.
- Dataset-processing/data/reports/recipes/ parse_report.json

Check for missing data, duplicate data, and invalid data
- dataset-processing/data/reports/recipes/ duplicate_report.json

Merge and remove duplicate recipe records.
- dataset-processing/data/reports/recipes/ merge_report.json

Standardize dish names and ingredient lists.
Remove records that do not meet the requirements.
- dataset-processing/data/reports/recipes/ clean_report.json

Re-check the data after processing.
Export recipes_cleaned.pkl.
- dataset-processing/data/reports/recipes/ verify_report.json

Extract the necessary calorie data from the USDA data.
- Dataset-processing/data/reports/USDA/energy_report.json

Standardize food names and nutrition data types.
- dataset-processing/data/reports/USDA/clean_report.json

Re-check the data after processing.
Export nutrition_cleaned.pkl.
- dataset-processing/data/reports/USDA/verify_report.json

### Step 3: Designing the Database
1. Determine the data that needs to be stored
2. Design the database
-	The user table stores user information:
    -	user_id: auto-incrementing primary key.
    -	cognito_sub: the user identifier from Amazon Cognito.
    -	username: display name.
    -	email: email address.
    -	created_at: account creation time.
-	The recipes table stores dish recipes:
    -	recipe_id: primary key.
    -	name: dish name.
    -	description: description.
    -	ingredients: the standardized ingredient list.
    -	ingredients_raw: the original ingredient list.
    -	steps: the preparation steps.
    -	servings: number of servings.
    -	serving_size: serving size.
    -	tags: classification tags.
    -	Fields for counting statistics and data status.
-	The nutrition table stores nutrition data:
    -	fdc_id: the primary key from USDA.
    -	food_name: food name.
    -	data_type: the USDA data type.
    -	calories: calorie amount.
-	The favorites table stores favorite dishes:
    -	user_id: foreign key referencing users.
    -	recipe_id: foreign key referencing recipes.
    -	created_at: the time the dish was added.
    -	Use the pair user_id and recipe_id as the composite primary key of the favorites table.
-	The search_history table stores search history:
    -	history_id: auto-incrementing primary key.
    -	user_id: the person who performed the search.
    -	keyword: the search keyword.
    -	total_result: the number of results returned.
    -	searched_at: the search time.
-	The chat_history table stores conversation history:
    -	history_id: auto-incrementing primary key.
    -	user_id: the person who sent the message.
    -	session_id: the conversation session ID.
    -	message: the message content.
    -	role: the role of the message sender.
    -	recipe_id: the related recipe, which may be left empty.
    -	created_at: the message creation time.
3. Set up the relationships between tables
-	Establish a one-to-many relationship between users and favorites.
-	Establish a one-to-many relationship between recipes and favorites.
-	Establish a one-to-many relationship between users and search_history.
-	Establish a one-to-many relationship between users and chat_history.
-	Establish an optional relationship between recipes and chat_history.
4. Define data constraints
- Set user_id, recipe_id, fdc_id, and the history IDs as primary keys.
- Set cognito_sub as unique to avoid syncing duplicate Cognito accounts.
- Set email as unique to avoid multiple accounts using the same email.
- Set the required fields to NOT NULL.
- Allow username to be empty when Cognito has not provided a display name.
- Allow recipe_id in chat_history to be empty when the conversation is not related to a specific recipe.
- Use the JSON type for:
    - ingredients
    - ingredients_raw
    - steps
    - tags
- Use utf8mb4 to fully support Unicode characters.
- Use MySQL's default time for the creation-date fields.
5. Design indexes
- Create idx_recipe_name for the recipes.name field.
- Create idx_favorite_recipe for the favorites.recipe_id field.
- Create the composite index idx_search_user_searched for:
    - search_history.user_id
    - search_history.searched_at
- Create the composite index idx_chat_user_created for:
    - chat_history.user_id
    - chat_history.created_at
- Create idx_chat_recipe for the chat_history.recipe_id field.
6. Create the SQL files
- Create the file schema.sql.
    - .\backend-api\database\seed\schema.sql
- Write the statements to create the six data tables.
- Declare primary keys, foreign keys, and constraints.
- Set up the database to use utf8mb4.
- Create the file indexes.sql.
    - .\backend-api\database\seed\ indexes.sql.
- Write the index-creation statements separately for convenient checking and deployment.
- Check the syntax of the two SQL files.
- Cross-check the SQL structure against the processed data.
- *Save both files in the backend's database folder.*
7. Verify the database design
- Check the table names and column names.
- Check the data type of each field.
- Check the length of the string fields.
- Check the primary keys and foreign keys.
- Check the UNIQUE and NOT NULL constraints.
- Check the relationships between tables.
- Check the ability to store JSON fields.
- Test the design with some sample data records.
- Confirm the structure is compatible with MySQL 8.
- Finalize schema.sql and indexes.sql for use when creating the database on Amazon RDS.
