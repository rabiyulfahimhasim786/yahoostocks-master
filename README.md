# yahoostocks-master
yahoostocks-master
Prerequisites
-------------

*   Internet
*   Google Cloud Platform account with Activated Billing account.

YAML Configuration to deploy FastAPI on Google App Engine
---------------------------------------------------------

Google Cloud Platform allows App Engine to perform deployment based on a configuration defined in a yaml file. For us to host the FastAPI on Google App Engine, the yaml configuration needs to have the following configuration.

runtime: python37
entrypoint: gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app

Our repository has a file _app.yaml_ at the root of the FastAPI project, that has the above specified yaml configuration to help deploy FastAPI to App Engine.

Project Setup
-------------

Login to Google Cloud Platform with the activated billing account.

### Create a New Project on Google Cloud Platform

Ensure to create a new project in GCP. For the new project being created, give Project name, Project ID, Billing account and click Create.

![Deploy FastAPI on GCP: Click New Project Button on GCP - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-01.1-Create-Project-1024x808.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 01.1 Create Project")

Deploy FastAPI on GCP: Click New Project Button on GCP – TutLinks

![Deploy FastAPI on GCP: Create New Project on Google Cloud Platform - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-01.2-New-Project-950x1024.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 01.2 New Project")

Deploy FastAPI on GCP: Create New Project on Google Cloud Platform – TutLinks

### Activate Cloud Shell

![Deploy FastAPI on GCP: Activate Cloud Shell - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-02-Activate-Cloud-Shell-1024x120.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 02 Activate Cloud Shell")

Deploy FastAPI on GCP: Activate Cloud Shell – TutLinks

Locate and click on the Activate Cloud Shell icon (having text _`>_`_) that is available in the top right of the logged in Google Cloud Platform home page.

### Clone Hello World FastAPI Repository from GitHub

The sample python based hello world FastAPI app is hosted on github repository. Type the following command in the Cloud Shell to clone the FastAPI repository on to GCP.

git clone -b fastapi-deploy-google-cloud-platform https://github.com/windson/fastapi.git

To learn more about the hello world app implemented using FastAPI, you can go through the detailed tutorial [here](https://tutlinks.com/create-and-deploy-fastapi-app-to-heroku/).

Set the current working directory to _fastapi_ by typing the following command.

cd fastapi

### Create the virtual environment

Create the [virtual environment](https://tutlinks.com/virtual-environments-in-python-for-beginners) named _env_ by typing the following command in cloud shell.

virtualenv env

### Activate the virtual environment

Activate the virtual environment by typing the following command in cloud shell.

source env/bin/activate

### Install Requirements for FastAPI

The project related libraries are located in _requirements.txt_. To install all the modules in one go, run the following command.

pip install -r requirements.txt

Preview FastAPI app on GCP App Engine
-------------------------------------

We will first do sanity and check if any issues we have running the app on Google App Engine. For that we will perform preview mode deployment of FastAPI. The preview deployment mode is not the standard and is just to double check and troubleshoot application deployment.

### Run the FastAPI app via Google Cloud Shell

In the Google Cloud Shell, execute the following command to run the hello world Flask app

gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app

*   `gunicorn` is the WSGI server to which we are configuring our application to run on, with the following configuration.
*   `-w 4` indicates that we need our application to run on gunicorn with four worker processes.
*   `-k uvicorn.workers.UvicornWorker` tells the gunicorn to run the application using _uvicorn.workers.UvicornWorker_ worker class.

*   `main:app` is our module main where our FastAPI() app is initialized.

You can also specify the host and port. But GCP will automatically figure them out and no extra configuration is needed for this demo.

### Understanding the output emitted by gunicorn command execution

![Deploy FastAPI on GCP: Understanding the output emitted by gunicorn command execution - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-05-Run-FastAPI-on-AppEngine-1024x363.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 05 Run FastAPI on AppEngine")

Deploy FastAPI on GCP: Understanding the output emitted by gunicorn command execution – TutLinks

We will understand output emitted by the execution of the aforementioned Gunicorn command.

Notice that the Gunicorn version 20.0.4 server is started as a first step of execution. This version of Gunicorn is same as the available version mentioned in requirements.txt of our repository.

In the second line that says _Listening at: http://127.0.0.1:8000_ indicates that our FastAPI app is running on localhost on port 8000 of the app engine.

Also we can notice that our FastAPI app has been spun up on the four worker processes having PIDs 529, 530, 531, 532. You may notice different PIDs based on the availability.

### Preview the running app in GCP

To see everything works fine, lets preview our hello world FastAPI app and see if it is running properly.

![Deploy FastAPI on GCP: Preview the running app in GCP - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-05.1-Web-Preview-on-Port-1024x247.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 05.1 Web Preview on Port")

Deploy FastAPI on GCP: Preview the running app in GCP – TutLinks

For preview, click on Preview button at the top right of the cloud shell as shown. Select change port and choose 8000 as our port on which our app can be previewed. We are choosing 8000 because our app is listening at http://127.0.0.1:8000/.

![Deploy FastAPI on GCP: Preview the running app output in GCP - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-05.2-Web-Preview-FastAPI-EndPoint-1024x386.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 05.2 Web Preview FastAPI EndPoint")

Deploy FastAPI on GCP: Preview the running app output in GCP – TutLinks

Now you will be taken to a new window where the page shows _{“message”:”Hello TutLinks.com”}_

Deploying FastAPI application on App Engine
-------------------------------------------

To deploy the FastAPI app on App Engine and access it via a custom domain or default your-proj-id.appspot.com, we need to create gcloud app and deploy our app.

### Create App on GCP

In the cloud shell, type the following command to create the app.

gcloud app create

![Deploy FastAPI on GCP: Create App on GCP - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-06.1-Create-app-1024x663.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 06.1 Create app")

Deploy FastAPI on GCP: Create App on GCP – TutLinks

You will be prompted to select the region. It is always a best practice to choose the region nearest to the geographical location where the consumers access this application. Doing so is expected increase the response times from our application.

### Deploy FastAPI app on GCP App Engine

GCP App Engine requires all the deployment configuration to be defined in a yaml file. Our repository has already has a yaml file named app.yaml that has the deployment configuration defined. Run the following command in Google Cloud Shell to deploy the FastAPI app to Google App Engine.

gcloud app deploy app.yaml

You will be prompted to continue. Press Y and hit enter to proceed with the deployment.

![Deploy FastAPI on GCP: Deploy FastAPI app on GCP App Engine - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-06.2-Deploy-FastAPI-app-to-AppEngine-1024x661.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 06.2 Deploy FastAPI app to AppEngine")

Deploy FastAPI on GCP: Deploy FastAPI app on GCP App Engine – TutLinks

Optionally you can also explicitly mention the project argument to the deployment command to be sure to mention the project where the app needs to be deployed using the following command.

gcloud app deploy app.yaml --project fastapi-tutlinks-demo

You must replace the fastapi-tutlinks-demo with the respective project id of the project you are working with in the above command.

### Browse the FastAPI deployed on GCP App Engine

Generally your app is deployed on the url that has the following format. your-project-id.appspot.com. In case you are not sure what the project id is, then type the following command to view your application in the web browser.

gcloud app browse

For testing we will send a request to the default endpoint via postman.

![Deploy FastAPI on GCP: Send a request our GCP hosted FastAPI app’s default endpoint on Postman - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-06.6-Send-Request-to-Default-FastAPI-endpoint-via-Postman-1-1024x754.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 06.6 Send Request to Default FastAPI endpoint via Postman 1")

Deploy FastAPI on GCP: Send a request our GCP hosted FastAPI app’s default endpoint on Postman – TutLinks

### Access Logs to troubleshoot

You can see what is happening like errors or info messages if at all being logged by our application running on the Google App Engine. To do that you can stream logs from the cloud shell command line by running the following command.

gcloud app logs tail -s default

Delete the Project
------------------

Once you are done with the deployment you may want to delete if you no longer need this project. For that navigate to IAM & Admin -> Settings -> SHUT DOWN (tab)

Key in the Project ID and hit SHUT DOWN button. The billing will be stopped effective immediately and the project will be deleted after 30 days.

![Deploy FastAPI on GCP: Delete the Project - TutLinks](https://tutlinks.com/wp-content/uploads/2022/09/fastapi-gcp-app-engine-deploy-07.2-Delete-Project-1024x624.png "Deploy FastAPI app on Google Cloud Platform - fastapi gcp app engine deploy 07.2 Delete Project")

Deploy FastAPI on GCP: Delete the Project – TutLinks

Congratulations 🎉, you have mastered how to deploy a hello world FastAPI app on Google Cloud Platform using App Engine. The full source code of the FastAPI app deployed to Google Cloud is available on [GitHub](https://github.com/windson/fastapi/tree/fastapi-deploy-google-cloud-platform).

Video Tutorial
--------------

The [video tutorial](https://bit.ly/3g0VwFE) covers very basic details like

*   Creating a project in GCP,

*   Accessing the Cloud shell,
*   Clone the repository on to Cloud Shell VM,
*   Do a Sanity hosting of the FastAPI on Cloud Shell VM and explain about worker processes

*   Create an App Engine
*   Deploy FastAPI on to App Engine
*   Check Logs of App Engine

*   Clean up and Shutdown Project once practiced the tutorial.
