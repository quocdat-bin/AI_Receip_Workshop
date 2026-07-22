---
title : "Deploying the Frontend & Authentication (Frontend & Auth)"
date : 2026-07-07
weight : 5
chapter : false
pre : " <b> 5.4 </b> "
---

### 1. Configuring the React Application
So that the Frontend can communicate smoothly with AWS services (Cognito, API Gateway) and be easy to extend in the future, the entire React codebase is organized tightly following a modular, feature-based model.

#### Here is the overall directory structure:
![endpoint diagram](/images/5-Workshop/5.5-Fronend/1.jpg)

#### Here is src/app/ (the application entry point)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/2.jpg)

-	App.jsx: The root component that orchestrates the entire application. 
-	ErrorBoundary.jsx: Handles unexpected system/UI errors. 
-	StyleGuide.jsx: The design standards page and sample components. 

#### src/routes/ (Routing)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/3.jpg)
-	AppRoutes.jsx: Configures the application's main routes. 
-	RequireAuth.jsx & RequireAdmin.jsx: Protected routes requiring login or Admin permissions. 
-	ScrollToTop.jsx: Automatically scrolls to the top of the page on route change. 
-	NotFoundPage.jsx: A fallback route for invalid paths.

#### src/layouts/ (Interface Layouts)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/4.jpg)
-	MainLayout.jsx: The main interface frame for regular users. 
-	AdminLayout.jsx: The interface frame dedicated to the admin pages. 
-	AuthLayout.jsx: The interface frame for authentication pages (Login, Register). 
-	components/: Includes Navbar, Sidebar, Footer, Logo, LanguageSwitcher. 

#### src/features/ (Core Features)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/5.jpg)
**Each subfolder represents an independent functional module:**
-	home/: The homepage with Banner, featured categories, AI suggestions, reviews, and nutrition tips. 
-	recipes/: Recipe management (recipe details, filters, AI ingredient search, ratings, comments, personal notes). 
-	calculator/: Tools for calculating nutrition, body metrics (such as BMI), and macro distribution for meals. 
-	cooking/: Interactive step-by-step cooking mode (CookingMode). 
-	wheel/: A random spin wheel feature (SpinWheel) to help answer the question "What should I eat today?". 
-	shopping/: Managing the ingredient shopping list (ShoppingList). 
-	collections/: Storing and managing personal recipe collections. 
-	nutrition/: Tracking daily nutrition metrics, history charts, and personal goals. 
-	chat/: The AI virtual assistant for cooking advice and recipe search. 
-	profile/: Managing the personal profile page, favorites list, and account settings. 
-	auth/: Login, register, and email verification pages (VerifyEmail, Callback). 
-	admin/: System administration pages (statistics, user, recipe, and category management). 

#### src/services/ (Data & API Handling)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/6.jpg)
-	Services: Contains the API communication files and processing logic per domain, such as RecipeService, AuthService, ChatService, NutritionService, ShoppingListService, AdminService, and more. 

#### src/ui/ (Shared UI Component Library)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/7.jpg)
-	Contains highly reusable basic UI components such as: Button, Card, Modal, Input, Tabs, Badge, Dropdown, Pagination, along with a charts/ folder for integrated charts (BarChart, LineChart, MacroDoughnut). 

#### src/hooks/ & src/contexts/ (State Management & Custom Hooks)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/8.jpg)

-	Contexts: Manage global state (AuthContext, FavoritesContext, ShoppingListContext). 
-	Hooks: Custom hooks that optimize logic (useAuth, useDebounce, useLocalStorage, useAsync, useFavorite). 

### 2.	Environment Configuration & Deployment
![endpoint diagram](/images/5-Workshop/5.5-Fronend/9.jpg)

-	package.json / package-lock.json: Manage libraries and the project's run scripts. 
-	vite.config.js: Configuration for the Vite bundler. 
-	amplify.yml: The deployment configuration file typically used for AWS Amplify. 
- .env.example: A template of the environment variables needed when setting up the project. 

### 3.	Implementation Steps

**Step 1: Prepare the Repository**
-	Make sure the project source code (including the folder containing the frontend) has been pushed to an online Git repository (such as GitHub, GitLab, Bitbucket, or AWS CodeCommit).
![endpoint diagram](/images/5-Workshop/5.5-Fronend/10.jpg)

**Step 2: Connect the Project to AWS Amplify**

-	Log in to the AWS Management Console, then search for and open the AWS Amplify service.
![endpoint diagram](/images/5-Workshop/5.5-Fronend/11.jpg)

-	On the overview page, choose Host your web app.
-	Select the source code provider (for example: GitHub) and click Next.
-	Find and select your repository (for example: nhi-ai/ai-recipe-finder) along with the deployment branch (for example: main).

![endpoint diagram](/images/5-Workshop/5.5-Fronend/12.jpg)

- Grant AWS Amplify permission to access your account and select the repository (ai-recipe-finder) and the branch you want to deploy (for example: main or master).

![endpoint diagram](/images/5-Workshop/5.5-Fronend/13.jpg)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/14.jpg)
- *Set up the environment variables *
![endpoint diagram](/images/5-Workshop/5.5-Fronend/18.jpg)
- Review one last time

**Step 3: Configure the Build (Build Settings)**
- Application setup: Set the application name under App name, then double-check the build command (npm run build) and the output directory (dist) under Build settings.
- Environment variable configuration: Expand the advanced settings to add the required environment variables
![endpoint diagram](/images/5-Workshop/5.5-Fronend/15.jpg)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/16.jpg)
**Step 4: Deploy**
- Review all the configuration information on the Overview/Review page.
- Click the confirm button to build and deploy to AWS's global CDN.

![endpoint diagram](/images/5-Workshop/5.5-Fronend/17.jpg)

-	Click Save and then click Save and deploy.
-	AWS Amplify will automatically:
  -	Pull the source code to the virtual server.
  -	Install the dependencies.
  -	Build the project into static files.
  - Publish it to AWS's global Content Delivery Network (CDN).
