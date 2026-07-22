---
title : "Building the Backend API with FastAPI"
date : 2026-07-07 
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---

### 1. Initialize the Backend Structure

- Create the backend folder.
- Create a Python virtual environment:
    - python -m venv .venv
- Activate the virtual environment on Windows:
    - .\venv\Scripts\Activate.ps1
- Install the main libraries
    - .\backend-api\requirements.txt
### 2. Build the ORM Models

- Create the User model mapped to the users table.
- Create the Recipe model mapped to the recipes table.
- Create the Nutrition model mapped to the nutrition table.
- Create the Favorite model mapped to the favorites table.
- Create the SearchHistory model mapped to the search_history table.
- Create the ChatHistory model mapped to the chat_history table.
- Declare the foreign keys and relationships between models.
- Import all models in app/models/__init__.py.

![endpoint diagram](/images/5-Workshop/5.3-Backend/13.jpg)

- Check that all ORM models can be imported successfully:
    - python -c "import app.models; print('All ORM models imported successfully')"


### 3. Build the Pydantic Schemas

- Create the condensed recipe data schema RecipeItem.
- Create the list response schema RecipeListResponse.
- Create the detail schema RecipeDetailResponse.
- Create the nutrition response schema NutritionResponse.
- Create schemas for users, favorites, and history when implementing the corresponding features.

![endpoint diagram](/images/5-Workshop/5.3-Backend/14.jpg)

### 4. Build the CRUD Layer

- Create a function to query the recipe list.
- Limit the number of records returned per request.
- Support pagination for the recipe list.
- Select only the necessary fields for the list API.
- Create a function to get recipe details by recipe_id.
- Create a function to get a random recipe.
- Create a function to get nutrition data by fdc_id.
- Create functions to check the existence of records.
- Use SQLAlchemy ORM or parameterized statements.
- Do not directly concatenate user data into the SQL query.
- Return None if no data is found.
- Do not allow CRUD to update or delete data in recipes and nutrition.
![endpoint diagram](/images/5-Workshop/5.3-Backend/15.jpg)

### 5. Build the Service Layer

- Create a service to handle recipe business logic.
- Call the CRUD layer to retrieve data from RDS.
- Convert query results into response schemas.
- Standardize the pagination structure.
- Check the page and limit boundaries.
- Handle the case where a recipe is not found.
- Create a service to handle nutrition data.
- Separate the business logic from the router for easier testing and maintenance.
- Do not return database errors directly to the user.
- Log the necessary errors but do not log passwords, tokens, or connection strings.

![endpoint diagram](/images/5-Workshop/5.3-Backend/16.jpg)

### 6. Build the Recipe & Nutrition API
- Build the API to get the recipe list: GET /recipes
- Build the API to get a random recipe: GET /recipes/random
- Build the API to get recipe details: GET /recipes/{recipe_id}
- Build the API: GET /nutrition/{fdc_id}

### 7. Initialize the FastAPI Application

- Create the file main.py.
- Initialize the application:
- Import the routers you built.
- Register the recipe router.
- Register the nutrition router.
- Create a status-check endpoint:
    - GET /
- Return the status that the backend is running.
- Only enable the Swagger documentation in the appropriate environment.
- Do not return the stack trace or internal information in error responses.

### 8. Run the Backend Locally

- Activate the virtual environment.
- Change to the backend folder.
- Start FastAPI:
    - uvicorn main:app --reload
- Access the check endpoint:
    - http://127.0.0.1:8000/

![endpoint diagram](/images/5-Workshop/5.3-Backend/17.jpg)

- Open Swagger UI:
    - http://127.0.0.1:8000/docs

![endpoint diagram](/images/5-Workshop/5.3-Backend/18.jpg)

- Check that the backend starts without import errors.

### 9. Test the Recipe API
- Call GET /recipes.
- Call GET /recipes/random.
- Call GET /recipes/{recipe_id} with an existing ID.
- Call GET /nutrition/167512
- Call GET /nutrition/{fde_id} with an existing ID.

![endpoint diagram](/images/5-Workshop/5.3-Backend/19.jpg)

### 13. Finalize the API
- Check that Cognito users are synced correctly into the users table on Amazon RDS.
- Finalize the Favorites API so users can add, view, and remove favorite recipes.

![endpoint diagram](/images/5-Workshop/5.3-Backend/34.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/35.jpg)
- Finalize the Search History API to save and retrieve search history.

![endpoint diagram](/images/5-Workshop/5.3-Backend/36.jpg)

- Finalize the Chat History API to save and manage conversation history.

![endpoint diagram](/images/5-Workshop/5.3-Backend/37.jpg)

- Finalize the remaining personal APIs and Backend features of the system.
![endpoint diagram](/images/5-Workshop/5.3-Backend/38.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/39.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/40.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/41.jpg)

- Require a Cognito access token for the APIs that contain personal data.
- Get the user identity from the access token verified by the Backend.
- Check that a user can only access and change data belonging to their own account.
- Test the APIs using Swagger UI and real data on Amazon RDS.
