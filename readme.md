# Build and deploy a URL Shortener using Django REST and Koyeb
### by Alex Merced

## Introduction

A URL shortener is a service that replaces long and hard-to-remember URLs with short and easy-to-remember ones. It does so by storing the long URL in a database and generating a short URL that redirects to the long one.

In this tutorial, we will explore how to create a URL shortener using Django REST and Postgres, and deploy it to production using Koyeb. By the end of this guide, you will have the knowledge and skills to develop your own URL shortening service, similar to popular platforms like [bit.ly](http://bit.ly/) or [tinyurl.com](http://tinyurl.com/), and take it live using Koyeb's platform.

We will delve into the inner workings of a URL shortener, leverage the capabilities of Django, harness the power of Django REST framework, and utilize the reliability of the Postgres database. Whether you're a Django enthusiast or just looking to build a practical web service, this tutorial will provide you with the necessary steps and insights to get you started.

Let's embark on this journey to create a powerful and scalable URL shortener with Django, Django REST framework, and Postgres, and witness it in action as we deploy it to production with Koyeb.

## Requirements

Before we dive into building our URL shortener with Django REST and Postgres, let's ensure that we have all the necessary tools and software in place. Here are the requirements for this project:

- **Python:** You will need Python version 3.8 or higher installed on your system. You can download Python from the official [Python website](https://www.python.org/downloads/).

- **Django:** We will be using Django as our web framework. Make sure you have Django version 3.2 or higher installed. You can install Django using pip with the following command:

  ```
  pip install django
  ```

- **Django REST framework:** Django REST framework (DRF) is a crucial part of our project. Install it using pip with the following command:

  ```
  pip install djangorestframework
  ```

These requirements are the foundation of our project, and they ensure that we can build our URL shortener effectively and efficiently. Once you have these components in place, we can proceed to set up our Django project and create the URL shortening application.

## Step 1: Creating a Local Django Project

Before we dive into building our URL shortener, we need to set up a local Django project environment. This step involves creating a dedicated virtual environment to manage project-specific dependencies and installing the required packages.

### 1.1 Create a Virtual Environment

First, let's create a virtual environment to isolate our project dependencies. Open your terminal and navigate to the directory where you want to create your Django project:

```bash
cd /path/to/your/project/directory
```

Now, create a virtual environment using the following command (replace myenv with your preferred environment name):

```bash
python -m venv myenv
```

This command will create a virtual environment named myenv in your project directory.

### 1.2 Activate the Virtual Environment
Next, you'll need to activate the virtual environment. Depending on your operating system, use the appropriate command:

On macOS and Linux:

```bash
source myenv/bin/activate
```

On Windows (Command Prompt):

```bash
myenv\Scripts\activate
```

On Windows (PowerShell):

```bash
.\myenv\Scripts\Activate.ps1
```

Once activated, you'll notice that your command prompt changes, indicating that you are now working within the virtual environment. This ensures that any packages you install are isolated from your system-wide Python installation.

### 1.3 Install Django and Django REST Framework
With the virtual environment active, we can now install Django and Django REST Framework. Run the following commands:

```bash
pip install django
pip install djangorestframework
```

These commands will install the latest versions of Django and Django REST Framework within your virtual environment.

### 1.4 Create a new Django Project

Now that we have our virtual environment set up and Django installed, we can create a new Django project. In this example, we'll name our project "urlshortener," but you can choose a different name if you prefer.

Open your terminal and make sure your virtual environment is still activated. Then, run the following command to create a new Django project:

```bash
django-admin startproject urlshortener
``` 
This command will create a new directory named "urlshortener" in your project directory, and it will contain the initial project structure. Here's what the resulting folder structure will look like:

```bash
your_project_directory/
├── urlshortener/           # Django project folder
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py         # Project settings file
│   ├── urls.py             # Project-level URL routing
│   └── wsgi.py
└── manage.py               # Django's command-line management utility
```

Your "urlshortener" Django project is now set up and ready for development.

## Step 2: Configuring DRF and Setting Up the Project

Now that we have our local Django project environment set up, the next step is to configure Django REST Framework (DRF) and configure our project to use it. DRF is a powerful toolkit for building Web APIs on top of Django, and it will be essential for creating our URL shortener application.

With your virtual environment still activated, 
To use DRF in your Django project, you need to configure it in your project's `settings.py` file. Here's how to do it:

Open your project's `settings.py` file, which is typically located in your project's main directory.

Locate the `INSTALLED_APPS` list in the `settings.py` file. Add 'rest_framework' to the list of installed apps. It should look like this:

```py
INSTALLED_APPS = [
    # ...
    'rest_framework',
    # ...
]
```

## Step 3: Setting Up the Postgres Database

In this step, we'll set up the Postgres database that our URL shortener application will use. You have two options: you can either set up a Postgres database locally or use a platform like Koyeb, Neon, or similar services to obtain a Postgres database.

### 3.1 Obtaining a Postgres Database

#### Local Setup (Optional)

If you prefer to set up the database locally, you can install PostgreSQL on your machine. You can download and install PostgreSQL from the official [PostgreSQL website](https://www.postgresql.org/download/).

#### Using a Platform

Alternatively, you can use cloud platforms like Koyeb, Neon, or others to provision a Postgres database. These platforms offer managed database services, which can simplify database management for your project.

### 3.2 Installing Postgres Drivers

Regardless of whether you choose a local or cloud-based Postgres database, you'll need to install Python drivers to connect to the database. The most common Python library for interacting with PostgreSQL is `psycopg2`. You can install it using pip:

```bash
pip install psycopg2
```

This library enables Django to communicate with the PostgreSQL database.

### 3.3 Configuring the Database
To configure the database for your Django project, we'll use the dj_database_url library. This library allows you to specify your database configuration using a URL format, making it easy to switch between different database providers.

Install dj_database_url using pip:

```bash
pip install dj-database-url
```

Open your project's settings.py file again.

Import dj_database_url at the top of the file:

```py
import dj_database_url
```

Locate the DATABASES setting in your settings.py file and replace it with the following code:

```py
import dj_database_url

# Use the DATABASE_URL environment variable to configure the database.
DATABASES = {
    'default': dj_database_url.config(
        conn_max_age=600,
        conn_health_checks=True,
    ),
}
```

### 3.4 Setting the DATABASE_URL environment variable

This configuration tells Django to use the `DATABASE_URL` environment variable to determine the database settings.

#### Define the variable locally

**Linux and macOS (Bash):**

```bash
export DATABASE_URL="your_database_url_here"
```

Replace "your_database_url_here" with the actual URL or connection string for your Postgres database.

**Windows (Command Prompt):**

```bash
set DATABASE_URL="your_database_url_here"
```

**Windows (PowerShell):**

```powershell
$env:DATABASE_URL="your_database_url_here"
```

Again, replace "your_database_url_here" with the actual URL or connection string for your Postgres database.

This will set the `DATABASE_URL` variable only for the active terminal session, it would need to be set again in any other terminal sessions.

#### Setting it up on your deployment platform

When deploying your app, make sure to set the `DATABASE_URL` variable on your deployment platform, we will cover how to do this with Koyeb later in this tutorial.

## Step 4: Creating the Model and the Serializer

Now that we've set up our Django project and database, it's time to define the core components of our URL shortener: the model and serializer. These components will allow us to manage URLs, track visits, and interact with our API.

### 4.1 URL Model

We'll start by defining the `URL` model, which represents the URLs we want to shorten. The model will have three fields:

- `hash` (string): A unique identifier for the shortened URL.
- `url` (string): The original URL that users want to shorten.
- `visits` (integer): A counter to keep track of the number of times the shortened URL has been visited.

In your Django project, create a new app or use an existing one. (Reminder: Django "apps" are subsections of functionality in a Django "project")

Creating a New App:
```py
django-admin startapp url
```

Registering the App in Settings.py:

```py
INSTALLED_APPS = [
    "...",
    "rest_framework",
    "url.apps.UrlConfig"
]
```



then define the `URL` model in the `models.py` file:


```python
from django.db import models

class URL(models.Model):
    hash = models.CharField(max_length=10, unique=True)
    url = models.URLField()
    visits = models.PositiveIntegerField(default=0)

    def __str__(self):
        return self.url
```

In this model, we use a CharField for the hash field, an URLField for the url field, and a PositiveIntegerField for the visits field. The hash field is unique to ensure that each shortened URL is unique.

### 4.2 URL Serializer
Next, let's create a serializer for the URL model. The serializer defines how the URL model should be serialized into JSON format and vice versa. This is essential for interacting with our API.

In your Django app, create a file named `serializers.py` if it doesn't already exist, and define the URLSerializer:

```py
from rest_framework import serializers
from .models import URL

class URLSerializer(serializers.ModelSerializer):
    class Meta:
        model = URL
        fields = ['hash', 'url', 'visits']
```

Here, we create a URLSerializer class that inherits from serializers.ModelSerializer. We specify the model as URL and list the fields we want to include in the serialized representation.

## Step 5: Creating All the Endpoints and Methods

In this step, we will create the API endpoints for our URL shortener application and implement the necessary methods behind each endpoint. These endpoints will allow users to shorten URLs, retrieve statistics, and access the original URLs via short hashes.

### 5.1 Redirecting to Original URL

**Endpoint:** `/url/:hash`

This endpoint will handle requests to access the original URL associated with a given hash. We will also increment the `visits` count for the URL.

**Implementation:**

In your `views.py` file, create a view to handle the redirection:

```python
from django.http import HttpResponseNotFound
from django.shortcuts import redirect
from .models import URL

def redirect_original_url(request, hash):
    try:
        url = URL.objects.get(hash=hash)
        url.visits += 1  # Increment visits count
        url.save()
        return redirect(url.url)
    except URL.DoesNotExist:
        return HttpResponseNotFound("Short URL not found")
```

Next, in your app's urls.py file (you may have to create `url/urls.py`), add a URL pattern to map requests to this view:

```py
from django.urls import path
from . import views

urlpatterns = [
    # ...
    path('url/<str:hash>/', views.redirect_original_url),
    # ...
]
```

### 5.2 Creating a New Short URL
**Endpoint:** /url

This endpoint will allow users to submit a long URL and receive a shortened URL in response.

**Implementation:**

In your views.py file, create a view to handle URL creation:

python
```py
from django.http import HttpResponseNotFound
from django.shortcuts import redirect
from .models import URL
from rest_framework.decorators import api_view
from django.http import JsonResponse
import hashlib

def redirect_original_url(request, hash):
    # ...

@api_view(['POST'])
def create_short_url(request):
    if 'url' in request.data:
        original_url = request.data['url']

        # Generate a unique hash for the URL
        hash_value = hashlib.md5(original_url.encode()).hexdigest()[:10]

        # Create a new URL object in the database
        url = URL.objects.create(hash=hash_value, url=original_url)

        # Return the shortened URL in the response
        return JsonResponse({'short_url': f'/url/{hash_value}/'}, status=201)

    return JsonResponse({'error': 'Invalid request data'}, status=400)
```

Add the URL pattern for this view in your app's urls.py file:

```py
urlpatterns = [
    # ...
    path('url/', views.create_short_url),
    # ...
]
```

### 5.3 Retrieving URL Statistics
**Endpoint:** /url/stats/:hash

This endpoint will provide statistics about a specific shortened URL, including the number of visits.

**Implementation:**

In your views.py file, create a view to retrieve URL statistics:

```python
from django.http import HttpResponseNotFound
from django.shortcuts import redirect
from .models import URL
from rest_framework.decorators import api_view
from django.http import JsonResponse
import hashlib
from .serializers import URLSerializer
from rest_framework.response import Response

def redirect_original_url(request, hash):
    ## ...

@api_view(['POST'])
def create_short_url(request):
    ## ...

@api_view(['GET'])
def get_url_stats(request, hash):
    try:
        url = URL.objects.get(hash=hash)
        serializer = URLSerializer(url)
        return Response(serializer.data)
    except URL.DoesNotExist:
        return Response({'error': 'Short URL not found'}, status=404)
```

Add the URL pattern for this view in your app's urls.py file:

```py
urlpatterns = [
    # ...
    path('url/stats/<str:hash>/', views.get_url_stats),
    # ...
]
```

### 5.4 Connecting the URLs
Finally, don't forget to include your app's URLs in your project's urls.py file. In the project's urls.py, include your app's URLs using the include function:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('url.urls')),  # Include your app's URLs here
]
```

### Step 5.5 Migrate

Run the following commands to migrate your database.

```bash
# create a migration for your url model
python manage.py makemigrations
# run the migration against the database
python manage.py migrate
```

## Step 6: Testing the App Locally

Before deploying your URL shortener to production, it's essential to thoroughly test it locally to ensure that all the endpoints and functionality work as expected. In this step, we'll cover how to test your app on your local development environment.

### 6.1 Running Your Django Development Server

To start testing your app, make sure your Django development server is up and running. Open your terminal, navigate to your project's root directory, and run the following command:

```bash
python manage.py runserver
```

This command will start the development server (`http://localhost:8000`), and you should see output indicating that the server is running.

### 6.2 Accessing the API Endpoints
With the development server running, you can now access the API endpoints you've created:

**Creating a New Short URL:** Use a tool like curl or Postman to send a POST request to /url with a JSON body containing a long URL. The server should respond with the shortened URL. Here is an example of what the request body should look like:

```json
{"url": "https://main.devnursery.com/"}
```

**Redirecting to Original URL:** Open a web browser or use a tool like curl to access a shortened URL in the format /url/:hash. You should be redirected to the original URL, and the visits count for that URL should increase.

**Retrieving URL Statistics:** Use a tool like curl to send a GET request to /url/stats/:hash. The server should respond with JSON data containing the URL's statistics.

### 6.3 Test Different Scenarios
To thoroughly test your app, consider various scenarios, such as:

- Creating multiple short URLs with different long URLs.
- Accessing non-existent short URLs to check error handling.
- Verifying that the visits count increases when accessing a short URL multiple times.
- Testing edge cases with long URLs, special characters, and various input data.

### 6.4 Debugging and Error Handling
While testing, if you encounter any issues or unexpected behavior, make use of Django's debugging tools, log messages, and error handling mechanisms. Review your code and logs to identify and fix any issues that arise during testing.

By thoroughly testing your app locally and addressing any issues you discover, you can ensure that it performs reliably and as expected when deployed to a production environment.

Once you are satisfied with the local testing and have resolved any issues, you can proceed to deploy your URL shortener to production using your chosen hosting platform, such as Koyeb or others.

Testing your app locally is a crucial step in the development process, as it allows you to catch and resolve issues early and provide a smooth user experience when your URL shortener goes live.

## Step 7: Deploying to Koyeb

[Koyeb Django Deployment Documentation](https://www.koyeb.com/docs/deploy/django#push-the-project-to-github)

Now that you have tested your Django application locally and it's working as expected, it's time to deploy it to production using Koyeb. Koyeb offers flexible deployment options, allowing you to choose between two methods: GitHub-based deployment or Docker-based deployment.

### Step 7.1: Deployment Setup

Before deploying your Django application, you need to set up your deployment environment. This includes installing Gunicorn, a WSGI HTTP server for serving your application, and generating a `requirements.txt` file to specify the dependencies for your project.

#### Installing Gunicorn

**Gunicorn** (short for Green Unicorn) is a popular WSGI HTTP server for running Python web applications. It's a recommended choice for deploying Django applications due to its stability and performance.

You can install Gunicorn using the following command, assuming you have Python and pip installed:

```bash
pip install gunicorn
```

#### Generating requirements.txt
The `requirements.txt` file lists all the Python packages and their versions that your Django application depends on. This file is essential for setting up the same environment in production as you have locally. Here's how to generate a requirements.txt file in different command-line environments:

Run the following command in your project's root directory to generate requirements.txt:

**Bash (Mac, Linux):**
```bash
pip freeze > requirements.txt
```

**Command Prompt (CMD - Windows):**
In the Windows Command Prompt, you can use the following command to generate requirements.txt:
```shell
pip freeze > requirements.txt
```

**PowerShell (Windows):**
In PowerShell, use the following command to generate requirements.txt:

```powershell
pip freeze > requirements.txt
```

This command will create a `requirements.txt` file containing a list of installed packages and their versions. It's important to keep this file up-to-date as you add or update dependencies in your Django project. When deploying to Koyeb or other platforms, you can use this file to ensure that the correct dependencies are installed in your production environment.

With Gunicorn installed and your requirements.txt file generated, you're ready to proceed with the deployment of your Django application on Koyeb or your chosen hosting platform.

### 7.2 runtime.txt

Make sure to specify the right version of python by creating a runtime.txt file with the following command:

```
echo "python-3.11.2" > runtime.txt
```

### Step 7.3: Allowed Hosts Configuration

To ensure the security and functionality of your Django application, you need to specify the allowed hosts that are permitted to access your application. This is achieved by configuring the `ALLOWED_HOSTS` setting in your Django project's `settings.py` file. Additionally, you should set the environment variable for `DJANGO_ALLOWED_HOSTS` during deployment, including your Koyeb URL.

### Updating `settings.py`

1. Open your Django project's `settings.py` file in your code editor.

2. Locate the `ALLOWED_HOSTS` setting in the file. By default, it may be commented out or set to an empty list.

3. Update the `ALLOWED_HOSTS` setting to dynamically read from an environment variable, allowing you to define the allowed hosts during deployment. Add the following code snippet:

```python
import os

ALLOWED_HOSTS = os.getenv("DJANGO_ALLOWED_HOSTS", "localhost,127.0.0.1,[::1]").split(",")
```

This code sets `ALLOWED_HOSTS` to the value of the `DJANGO_ALLOWED_HOSTS` environment variable if it is defined. If the environment variable is not set, it falls back to a default list of allowed hosts.

#### Setting the Environment Variable
During deployment on Koyeb, it's essential to set the `DJANGO_ALLOWED_HOSTS` environment variable to include your Koyeb URL. The Koyeb URL follows the pattern `<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app`, where `<YOUR_APP_NAME>` is the name of your app, and `<YOUR_KOYEB_ORG>` is your Koyeb organization name.

### 7.4 GitHub-Based Deployment

GitHub-based deployment is a convenient way to deploy your Django application to Koyeb. Follow these steps to deploy using GitHub:

1. **Create a GitHub Repository:** If you haven't already, create a GitHub repository to host your Django application's code.

2. **Push Your Code to GitHub:** Commit your application code to the GitHub repository you created in step 1.

3. **Set Up Deployment in Koyeb:** In the Koyeb control panel, create a new app and select GitHub as the deployment option. Choose the repository and branch that contain your application code.

4. **Configure Build and Deployment Settings:** Customize your deployment settings, including the run command (e.g., `gunicorn url_project.wsgi`),  and set environment variables as needed. The variables that should be set include.

```bash
# Database URL
DATABASE_URL=postgres://...
# Disabling Collect Static since it is just an API
DISABLE_COLLECTSTATIC=1
# Django ALLOW_HOSTS
DJANGO_ALLOWED_HOSTS=<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app
```

**NOTE**: Make sure your deployment database has been migrated

5. **Deploy Your App:** Click the "Deploy" button in the Koyeb control panel. Koyeb will automatically build and deploy your Django application based on changes detected in your GitHub repository.

6. **Access Your Deployed App:** Once the deployment is complete, you can access your Django application by clicking the provided URL ending with `.koyeb.app`.

### Step 7.5: Docker-Based Deployment

If you prefer Docker-based deployment, you can containerize your Django application and deploy it to Koyeb using a pre-built Docker container. Here's how to do it:

#### Dockerizing Your Django App

1. **Create a Dockerfile:** In your project's root directory, create a `Dockerfile` to define the Docker image for your Django application. Below is an example of a `Dockerfile` for a Django app:

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.11

# Set up a working directory
WORKDIR /app

# Copy your Django project files into the container
COPY . /app

# Install any dependencies using pip (requirements.txt should be present)
RUN pip install -r requirements.txt

# Expose the port your application will run on
EXPOSE 8000

# Define the command to start your Django app
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

**Build and Push the Docker Image:** Build the Docker image using the docker build command and then push it to a container registry, such as Docker Hub or a private repository. Replace `<your_image_name>` with the desired name for your Docker image and `<your_tag>` with a version tag.

```bash
# Build the Docker image
docker build -t <your_image_name>:<your_tag> .

# Push the Docker image to a registry
docker push <your_image_name>:<your_tag>
```

#### Configure Environment Variables
Configure Environment Variables: In the Koyeb control panel, set environment variables required for your Django application. Ensure that you define the DJANGO_ALLOWED_HOSTS variable, which should include your Koyeb URL in the format `<YOUR_APP_NAME>-<YOUR_KOYEB_ORG>.koyeb.app`.

#### Deploy from Docker Image
Deploy from Docker Image: In the Koyeb control panel, create a new app and choose the "Deploy from a Docker Image" option. Provide the URL of your Docker image in the container registry, including the image name and tag.

#### Configure Deployment Settings
Configure Deployment Settings: Customize your deployment settings, such as specifying the run command and the port your Django application will listen on.

#### Deploy Your App
Deploy Your App: Click the "Deploy" button to initiate the deployment process. Koyeb will pull your Docker image from the registry and deploy your Django application.

#### Access Your Deployed App
Access Your Deployed App: Once the deployment is complete, you can access your Django application using the URL provided by Koyeb, which typically ends with ``.koyeb.app``.

Choose the deployment method that best aligns with your infrastructure and preferences. GitHub-based deployment simplifies the process if your code is hosted on GitHub, while Docker-based deployment offers greater control over your application's environment and dependencies.

With your Django application successfully deployed to Koyeb as a Docker container, it becomes accessible to users on the internet. You can monitor its performance, scale it as needed, and continue enhancing and expanding your web application.
