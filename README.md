# Python-Scripts
Here's a step-by-step guide to help you deploy a Flask or FastAPI application on Azure Web Apps with CI/CD using GitHub Actions.

### Step 1: Create the Application

We'll start by creating a simple Flask or FastAPI application.

#### Flask Application

1. Create a new directory for your project and navigate into it:
   ```bash
   mkdir flask_app
   cd flask_app
   ```

2. Create a virtual environment and activate it:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install Flask:
   ```bash
   pip install flask
   ```

4. Create a file named `app.py` with the following content:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def hello():
       return "Hello, Azure Web Apps!"

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=80)
   ```

#### FastAPI Application

1. Create a new directory for your project and navigate into it:
   ```bash
   mkdir fastapi_app
   cd fastapi_app
   ```

2. Create a virtual environment and activate it:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install FastAPI and Uvicorn:
   ```bash
   pip install fastapi uvicorn
   ```

4. Create a file named `main.py` with the following content:
   ```python
   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/")
   async def read_root():
       return {"message": "Hello, Azure Web Apps!"}

   if __name__ == '__main__':
       import uvicorn
       uvicorn.run(app, host='0.0.0.0', port=80)
   ```

### Step 2: Create a GitHub Repository

1. Initialize a Git repository:
   ```bash
   git init
   ```

2. Add all files and commit:
   ```bash
   git add .
   git commit -m "Initial commit"
   ```

3. Create a new repository on GitHub and push your code to it:
   ```bash
   git remote add origin <your_github_repository_url>
   git branch -M main
   git push -u origin main
   ```

### Step 3: Set Up Azure Web App

1. Go to the [Azure Portal](https://portal.azure.com/).

2. Create a new Azure Web App:
   - Select your subscription and resource group.
   - Provide a name for your web app.
   - Choose a runtime stack (Python 3.x).

3. Once the web app is created, go to its settings and note the deployment credentials.

### Step 4: Configure GitHub Actions for CI/CD

1. In your GitHub repository, create a `.github/workflows` directory.

2. Create a file named `azure_webapp.yml` inside the `.github/workflows` directory with the following content:

```yaml
name: Deploy to Azure Web App

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.x'

    - name: Install dependencies
      run: |
        python -m venv venv
        source venv/bin/activate
        pip install -r requirements.txt

    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: '<your_web_app_name>'
        slot-name: 'production'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
```

3. Create a `requirements.txt` file in your project directory and add the required dependencies. For Flask:
   ```plaintext
   Flask
   ```
   For FastAPI:
   ```plaintext
   fastapi
   uvicorn
   ```

4. In the Azure Portal, go to the "Deployment Center" of your web app and generate the publish profile. Download the profile and add it as a secret in your GitHub repository. Name the secret `AZURE_WEBAPP_PUBLISH_PROFILE`.

### Step 5: Confirm Deployment

1. Push the changes to your GitHub repository:
   ```bash
   git add .
   git commit -m "Add GitHub Actions workflow"
   git push origin main
   ```

2. GitHub Actions will trigger the workflow and deploy your application to Azure Web Apps.

3. Access your web app through the provided URL and ensure it returns "Hello, Azure Web Apps!".

### Submission Guidelines

- Share the GitHub repository containing your Flask/FastAPI application code.
- Include the `.github/workflows/azure_webapp.yml` configuration file.
- Ensure your web app is accessible and returning the expected response.

### Example GitHub Repository

[Example GitHub Repository](https://github.com/username/azure-webapp-example) (Replace with your actual repository URL)

Following these steps should help you successfully deploy a Flask or FastAPI application on Azure Web Apps with CI/CD using GitHub Actions. If you encounter any issues or have questions, feel free to ask!
