---
title : "Deploying the FastAPI Backend to Amazon EC2"
date : 2026-07-07 
weight : 4
chapter : false
pre : " <b> 5.2.4 </b> "
---

### 1. Deploy the FastAPI Backend to Amazon EC2
**Step 1. Prepare for Deployment**
- Check that the backend runs stably on the local machine.
- Confirm the basic APIs work:
    - GET /
    - GET /recipes
    - GET /recipes/random
    - GET /recipes/{recipe_id}
    - GET /nutrition/{fdc_id}
- Check that requirements.txt contains all libraries.
- Check that .gitignore does not allow committing:
    - .env
    - .venv
    - .pkl files
- Dataset
- Certificates
- Log files
- Push the backend source code to a private GitHub repository.
- Check that Amazon RDS is in the Available state.
- Choose the EC2 deployment region to be the same region as RDS

**Step 2. Create an Amazon EC2 Instance**

- Log in to the AWS Management Console.
- Go to the Amazon EC2 service.
- Choose Launch instance.

![endpoint diagram](/images/5-Workshop/5.3-Backend/20.jpg)

- Name the EC2 instance.
- Choose the Ubuntu Server operating system.
- Choose an architecture compatible with the project's Python libraries.

![endpoint diagram](/images/5-Workshop/5.3-Backend/21.jpg)

- Choose an instance type suitable for the testing environment.
- Create or choose a key pair for SSH login.
- Download the private key file to your machine.
- Store the private key file in a safe location.

![endpoint diagram](/images/5-Workshop/5.3-Backend/22.jpg)

- Choose a VPC suitable for the system architecture.
- Configure an appropriate amount of storage.
- Check the estimated total cost before creating.
- Choose Launch instance.
- Wait for the EC2 to change to the Running state.
- Check that the instance's status checks are complete.

![endpoint diagram](/images/5-Workshop/5.3-Backend/23.jpg)

### 2. Configure the Security Group for EC2
- Open the Security Group attached to the EC2. Add a rule

![endpoint diagram](/images/5-Workshop/5.3-Backend/24.jpg)

- Review all inbound and outbound rules.
- Confirm that only the necessary ports are made public.

### 3. Assign an Elastic IP
- Go to the Elastic IP addresses section in EC2.
- Choose Allocate Elastic IP address.
- Create an Elastic IP address.
- Select the newly created Elastic IP.
- Choose Associate Elastic IP address.
- Attach the Elastic IP to the EC2 instance.

![endpoint diagram](/images/5-Workshop/5.3-Backend/25.jpg)

- Check that the IP address has been associated.

![endpoint diagram](/images/5-Workshop/5.3-Backend/26.jpg)

- Use the Elastic IP instead of the dynamic public IP.


### 4. Connect to EC2 via SSH

- Open a terminal in the folder containing the private key.
- Restrict the read permission on the key file
![endpoint diagram](/images/5-Workshop/5.3-Backend/27.jpg)

- Connect to the EC2:
![endpoint diagram](/images/5-Workshop/5.3-Backend/28.jpg)

- Confirm the server fingerprint on the first connection.
Check that the login succeeds.


### 5. Update the EC2 Operating System
- Update the package list: sudo apt update
- Install the necessary updates: sudo apt upgrade -y
- Install the basic tools: sudo apt install -y git python3 python3-pip python3-venv nginx
- Check the Python version: python3 --version
- Check Git: git --version
- Check Nginx: nginx -v
- Check the Nginx status: sudo systemctl status nginx
- Access the Elastic IP in a browser.
- Confirm the default Nginx page displays successfully.

### 6. Push the Backend Source Code to EC2
- cd /home/ubuntu
- Clone the repository: git clone repository-url
- cd ai_recipe_finder
- Check the current branch: git branch --show-current
- Switch to the deployment branch if needed.
- Check the source code: git status
- Confirm the backend folder and requirements.txt already exist.

### 7. Create the Python Environment on EC2
- cd /home/ubuntu/ai_recipe_finder/backend-api
- Create the virtual environment: python3 -m venv .venv
- source .venv/bin/activate
- python -m pip install --upgrade pip
- Install the libraries: pip install -r requirements.txt
- pip list
- Check all ORM models:
- python -c "import app.models; print('All ORM models imported successfully')"

### 8. Configure Environment Variables on EC2
- Create a .env file in the backend folder: nano .env
- Declare the RDS connection information:
- DB_HOST=rds-endpoint
- DB_PORT=3306
- DB_NAME=food_ai_db
- DB_USER=backend-database-user
- DB_PASSWORD=database-password
- Declare the necessary application configuration.
- Declare an appropriate list of CORS origins.
- Restrict the read permission on the file: chmod 600 .env
- git status
- Confirm .env does not appear in the list of files staged for commit.

### 9. Allow EC2 to Connect to Amazon RDS
- Open the Amazon RDS Security Group.
- Choose Edit inbound rules.
- Add a MySQL/Aurora rule:
- Type: MySQL/Aurora
- Protocol: TCP
- Port: 3306
- Source: the EC2 Security Group

![endpoint diagram](/images/5-Workshop/5.3-Backend/29.jpg)

- Save the Security Group.
- Check that RDS is in the Available state.
- From EC2, check the ability to open a connection to RDS.
- Confirm the backend connects to the correct food_ai_db schema.
- Check the queries reading data from recipes and nutrition.

### 10. Run FastAPI on EC2 as a Test
- source .venv/bin/activate
- Start Uvicorn to test:
- uvicorn main:app --host 0.0.0.0 --port 8000
- Check that no import errors appear in the terminal.
- Check that the backend connects to RDS successfully.
- If port 8000 is temporarily open, access:
- http://<elastic-ip>:8000/
- Check Swagger:
- http://<elastic-ip>:8000/docs
- Try calling the recipe and nutrition APIs.
- Stop the test process with Ctrl + C.
- After a successful test, switch to running with systemd.


### 11. Create a systemd Service for the Backend
Create the service: sudo nano /etc/systemd/system/recipe-backend.service
Declare the configuration:

*[Unit]*
- Description=AI Recipe Finder FastAPI Backend
- After=network.target

*[Service]*
- User=ubuntu
- Group=www-data
- WorkingDirectory=/home/ubuntu/ai_recipe_finder/backend
- EnvironmentFile=/home/ubuntu/ai_recipe_finder/backend/.env
- ExecStart=/home/ubuntu/ai_recipe_finder/backend/.venv/bin/uvicorn main:app --host 127.0.0.1 --port 8000
- Restart=always
- RestartSec=5

*[Install]*
- WantedBy=multi-user.target
- Double-check the repository path and virtual environment.
- Reload the systemd configuration: sudo systemctl daemon-reload
- Start the backend: sudo systemctl start recipe-backend
- Allow the backend to start automatically with EC2: sudo systemctl enable recipe-backend
- Check the status: sudo systemctl status recipe-backend
- Check that the backend is listening on 127.0.0.1:8000.
- Confirm the service is in the active (running) state.


### 12. Check the Backend Logs

- View the most recent logs:
- sudo journalctl -u recipe-backend -n 100 --no-pager
- Check for errors

### 13. Configure the Nginx Reverse Proxy
- Create the Nginx configuration file:
- sudo nano /etc/nginx/sites-available/recipe-backend
- Add the reverse proxy configuration:

```nginx
server {
    listen 80;
    server_name <domain-or-elastic-ip>;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_connect_timeout 30s;
        proxy_read_timeout 60s;
    }
}
```
- Enable the configuration:
- sudo ln -s /etc/nginx/sites-available/recipe-backend \
-   /etc/nginx/sites-enabled/recipe-backend
- Delete or disable the default site if it causes a conflict.
- Check the syntax: sudo nginx -t
- Restart Nginx: sudo systemctl restart nginx
- Check the status: sudo systemctl status nginx
- Access the backend via:
- http://elastic-ip/
- Confirm the request is forwarded by Nginx to FastAPI.

### 14. Configure CORS for the Frontend
- Get the Amplify base URL from the frontend member.
- Configure the CORS origin in the backend environment variables.
- For example:
- CORS_ORIGINS=http://localhost:5173,https://<amplify-domain>
- Only declare the scheme and domain.
- Do not add:
- /callback
- /login
- Restart the backend after editing .env: sudo systemctl restart recipe-backend
- Check a request from localhost.
- Check a request from Amplify.
- Check that an origin not in the list does not receive a CORS header.
- Distinguish CORS errors from authentication errors or network connection errors.

### 15. Test the APIs in the EC2 Environment
- Check the status endpoint:
- GET /
- Check the recipe list:
- GET /recipes
- Check a random recipe:
- GET /recipes/random
- Check recipe details:
- GET /recipes/{recipe_id}
- Check nutrition data:
- GET /nutrition/167512
- Confirm Swagger displays correctly in the permitted environment.
- Confirm the API does not return internal connection information.

### 16. Update the Backend on EC2
- Commit and push the changes from the development machine to GitHub.
- Connect to EC2 via SSH.
- Change to the repository: cd /home/ubuntu/ai_recipe_finder
- Check the status before updating: git status
- Do not run git pull if EC2 has undefined uncommitted local changes.
- Pull the new source code: git pull origin main
- Activate the virtual environment:
- cd backend
- source .venv/bin/activate
- Check the model and application imports.
- Restart the backend: sudo systemctl restart recipe-backend
- Check the service status.
- Check the logs.
- Call the important APIs again.
- Confirm the new version has been deployed.
