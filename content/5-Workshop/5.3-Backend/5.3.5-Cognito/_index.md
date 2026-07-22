---
title : "Configuring Amazon Cognito, User Authentication, and API Security"
date : 2026-07-07 
weight : 5
chapter : false
pre : " <b> 5.2.5 </b> "
---

### 1. Define the Authentication Flow
- Use an Amazon Cognito User Pool to manage user accounts.
- The Frontend is responsible for:
    -	Registering an account.
    -	Verifying the email.
    -	Logging in.
    -	Receiving tokens from Cognito.
    -	Sending the access token to the Backend.
- The Backend is responsible for:
    -	Receiving the access token.
    -	Verifying the JWT signature.
    -	Checking the issuer.
    -	Checking the token expiration.
    -	Checking token_use.
    -	Checking the App Client ID.
    -	Retrieving user information.
    -	Syncing the user into the users table.
    -	Amazon RDS does not store user passwords; passwords are managed by Amazon Cognito.

### 2. Create an Amazon Cognito User Pool
- Log in to the AWS Management Console.
- Go to the Amazon Cognito service.
- Choose Create user pool.
- Choose email as the sign-in method.
- Set email as a required attribute.
- Enable self-registration if you allow users to create accounts.
- Enable email verification.
- Configure Cognito to send the verification code to the user's email.
- Set up an appropriate password policy:
    -	Minimum length.
    -	Uppercase letters.
    -	Lowercase letters.
    -	Digits.
    -	Special characters if needed.
- Configure the expiration time of the verification code.
- Complete the User Pool creation.

![endpoint diagram](/images/5-Workshop/5.3-Backend/30.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/31.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/32.jpg)

### 3. Create a Cognito App Client
- Open the newly created User Pool.
- Go to the App clients section.
- Choose to create a new App Client.
- Give it a name identifying the frontend application.
- Choose the application type suitable for a Single-page application.
- React running in the browser cannot securely protect a client secret.
- Enable the Authorization Code Grant.
- Use the Authorization Code Flow combined with PKCE.

![endpoint diagram](/images/5-Workshop/5.3-Backend/33.jpg)

### 4. Configure the Cognito Domain
- Open the domain management section of the User Pool.
- Create a Cognito Hosted UI domain.
- Choose a unique domain prefix.
- Check that the domain is accessible.
- The issuer used to verify the JWT has the form:
- https:cognito-idp.region.amazonaws.com/user-pool-id

### 5. Configure the Callback URL and Sign-out URL
- Collect the URLs from the frontend member.
- Configure the callback URL for the local environment:
- http://localhost:5173/callback
- Configure the sign-out URL for the local environment:
- http://localhost:5173/login
- Configure the Amplify callback URL:
- https://main.d2ykhh6couuz7m.amplifyapp.com/callback
- Configure the Amplify sign-out URL:
- https://main.d2ykhh6couuz7m.amplifyapp.com/login
- Save the App Client configuration after updating.
- Wait for the configuration to be applied before testing.

### 6.  Configure the Backend Environment Variables
- Add the Cognito variables to the Backend's .env:
- COGNITO_REGION=ap-southeast-1
- COGNITO_USER_POOL_ID=user-pool-id
- COGNITO_APP_CLIENT_ID=app-client-id
- COGNITO_DOMAIN=https://cognito-domain
- Build the issuer from the region and User Pool ID:
- https://cognito-idp.ap-southeast-1.amazonaws.com/user-pool-id
- Build the JWKS URL:
- https://cognito-idp.ap-southeast-1.amazonaws.com/user-pool-id/.well-known/jwks.json

### 7. Install the Backend Authentication Libraries
- Add the libraries needed to handle JWTs and send HTTP requests.
- You can use:
- PyJWT
- cryptography
- httpx
- Update requirements.txt.
- Install the dependencies in the virtual environment:
- pip install -r requirements.txt
- Check that the libraries import successfully.

### 8. Build the /auth/me API
- Create an authentication router with the prefix:
- /auth
- Create the endpoint:
- GET /auth/me
- Protect the endpoint with a Cognito JWT dependency.
- The Frontend sends the request:
- GET /auth/me
- Authorization: Bearer access_token
- If the token is valid:
- Verify the user.
- Find or create a record in the users table.
- Return the current user's information.
- If the bearer token is missing, expired, or invalid, return HTTP 401.
- If an internal error occurs while syncing the user, do not return the stack trace.
- Register the auth router in main.py.
- Check that Swagger displays the appropriate bearer authentication mechanism.

![endpoint diagram](/images/5-Workshop/5.3-Backend/33_2.jpg)

### 9. Sync Users with Amazon RDS
- When a user logs in successfully for the first time and calls /auth/me, the Backend creates a record in the users table.
- Look up the user by cognito_sub.
- If it does not exist yet, create:
- cognito_sub
- username
- email
- created_at
- If it already exists, reuse the existing user_id.
- Check that the email does not match another account.
- Check the data directly on RDS after the first login.
- Confirm there is only one record for one cognito_sub.
- Call /auth/me again.
- Confirm the number of records does not increase.
- Confirm the user_id is kept unchanged.

### 10. Configure CORS for Authentication
- Update the Backend environment variables:
- CORS_ORIGINS=http://localhost:5173,https://main.d2ykhh6couuz7m.amplifyapp.com
- Restart the Backend after changing CORS:
- sudo systemctl restart recipe-backend
- Check the preflight request from the frontend.
- Check the request from Amplify.

### 11. Test /auth/me
- Call the API with a valid access token:
- GET /auth/me
- Authorization: Bearer <access_token>
- Confirm the Backend returns HTTP 200.
- Check that the response has the correct:
- user_id
- cognito_sub
- username
- email
- created_at
- Check the users table on RDS.
- Confirm a new user was created.
- Call /auth/me again with the same account.
- Confirm no duplicate user is created.
- Confirm the user_id does not change.
- Try a token with a modified payload.
- Confirm the Backend returns HTTP 401.
- Try an expired token.
- Confirm the Backend returns HTTP 401.

### 12. Deploy the Backend with Authentication to EC2
- Commit the finalized source code on the development machine.
- Push the code to GitHub.
- Connect to EC2 via SSH.
- Change to the repository: cd /home/ubuntu/ai_recipe_finder
- Check local changes: git status
- Pull the new source code: git pull origin main
- Change to the Backend: cd backend
- Activate the virtual environment: source .venv/bin/activate
- Update the Cognito variables in .env.
- Check the imports:
- python -c "import app.models; print('All ORM models imported successfully')"
- Check that the application can be imported:
- python -c "from main import app; print('FastAPI app imported successfully')"
- Restart the service:
- sudo systemctl restart recipe-backend
- Check the status:
- sudo systemctl status recipe-backend
- Check the public endpoint.
- Check that /auth/me without a token returns 401.
- Check that /auth/me with a valid access token returns 200.
- Confirm the new version has been deployed successfully.
