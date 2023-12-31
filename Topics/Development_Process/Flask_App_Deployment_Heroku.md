# Deploying a Flask Application on Heroku

## Table of Contents
### [Introduction](#introduction)
### [Before Getting Started](#before-getting-started)
### [Steps](#steps)
### [Common Issues](#common-issues)

----

## Introduction

This simple guide will walk you through the steps of deploying a Flask web application on Heroku. Heroku is a popular cloud-based platform that allows for the simple deployment and management of web applications. 

----

## Before Getting Started

Before you begin, ensure you have the following:

- An account with Heroku. Here is where you can sign up: https://www.heroku.com
- A Git repository that contains your Flask application.
- Heroku CLI installed. Here is where you can download it: https://devcenter.heroku.com/articles/heroku-cli

----

## Steps

**1) Logging into Heroku Through the Terminal:**

Open your terminal and navigate to your Git repository's root directory. Then, type the following into the terminal and follow the instructions to log into Heroku:

    heroku login

**2) Creating a requirements.txt File:**

If you don't have one already, add a file into your project's directory named "requirements.txt" that includes all the requirements for your application. The simplest way to create one is by typing the following command:

    python3 -m pip freeze > requirements.txt

**3) Installing Gunicorn:**

If you don't already have Gunicorn installed, install it by typing the following command into the terminal:

    pip3 install gunicorn

Gunicorn serves as a connection between your Flask app and Heroku's dyno and allows for improved reliability in terms of the deployment of Python applications.

**4) Creating a Procfile:**

Create a file named "Procfile" in your project's root directory. This file is required for Heroku to deploy your application. In your "Procfile", add the following line:

    web: gunicorn app:app

**5) Creating a New Heroku Application:**   

While in your Git repository's root directory, type the following to create a new app on Heroku using the terminal:

    heroku create name-of-your-app
    
Remember to replace "name-of-your-app" with a name for your Heroku app. This name must be unique across all of Heroku.

**6) Pushing Changes:**   

Ensure that all the changes are committed and pushed to your repository.

**7) Deploying the Application:**  

You can now deploy your Flask application to Heroku by typing the following command into the terminal:

    git push heroku master

**8) Opening the Application:**  

You can now open the deployed application in your default browser by typing the following command into the terminal:

    heroku open

**Additional Comments:**  

If you prefer a more visual approach to deploying your Flask application on Heroku, you could use Heroku's web interface instead of the CLI. While this method is more user-friendly, it has a few drawbacks including limited control over advanced configuration settings and dependency on a web browser.

This method leads to manual deployment through which the command "git push heroku master" triggers the deployment. This gives you full control over what and when to deploy. Alternatively, you could also connect a GitHub repository to your Heroku app through their website to set up automatic deployment. When any changes are pushed to your repository, Heroku will detect them and deploy the app again right away.   

----

## Common Issues

**Application Filename is Not "app.py":**

Ensure that your Flask application's filename is either "app.py" and that the Flask application's instance within that file is stored in a variable called "app". Otherwise, you can change your Procfile to reflect the file and variable names you used by changing the line in it to the following:

    web: gunicorn your_filename:instance_variable_name

Remember to replace "your_filename" and "instance_variable_name" with the names you used.

**Other Issues with Deployment:**

Firstly, ensure that your "requirements.txt" file is created and is up to date. Also, ensure that your Procfile is free of any typos.
If you still encounter any issues with deployment, checking the Heroku logs created can be very helpful. To view real-time logs, type the following into the terminal:

    heroku logs --tail

Some logs Heroku provides include:

- Error logs which contain error messages caused from the execution of your application's code
- Application logs which contain information about the execution of your application's code including print statements
- Router logs which contain information about incoming traffic and issues with request handling.

----

For More Detailed Steps for the Creation and Deployment of a New Flask Application: 
- https://realpython.com/flask-by-example-part-1-project-setup/


