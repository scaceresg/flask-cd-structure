# Project Scaffold for a Flask API Continuous Delivery using GCP's App Engine

**Contents**:

- [Project Scaffold for a Flask API Continuous Delivery using GCP's App Engine](#project-scaffold-for-a-flask-api-continuous-delivery-using-gcps-app-engine)
  - [Create and Deploy a Flask Application in Google Cloud's Environment](#create-and-deploy-a-flask-application-in-google-clouds-environment)
    - [Project Scaffold](#project-scaffold)
    - [Test the Python Application Locally](#test-the-python-application-locally)
    - [Set Up and Deploy the Flask API in the App Engine Service](#set-up-and-deploy-the-flask-api-in-the-app-engine-service)
  - [Set up Continuous Delivery using Cloud Build](#set-up-continuous-delivery-using-cloud-build)


## Create and Deploy a Flask Application in Google Cloud's Environment

### Project Scaffold

Create a project repository in GitHub and add the following 
files:

* **Makefile**: to build the application steps:

```
install:
    pip install -r requirements.txt
```

* **requirements.txt**: file with the required Python 
dependencies:

```
flask
```

* **main.py**: simple Flask app in Python:

```
from flask import Flask
from flask import jsonify
app = Flask(__name__)

@app.route('/')
def hello():
    """Return a friendly HTTP greeting."""
    print("I am inside hello world")
    return 'Hello World!'

@app.route('/echo/<name>')
def echo(name):
    print(f"This was placed in the url: new-{name}")
    val = {"new-name": name}
    return jsonify(val)

if __name__ == '__main__':
    app.run(host='127.0.0.1', port=8080, debug=True)
```

### Test the Python Application Locally

* Clone the GitHub project into your local directory. Then, 
create and activate a Python virtual environment:

    - `virtualenv /.flask-cd`

    - `source /.flask-cd/bin/activate`

* Install the dependencies: `make install`

* Test the Flask app by running `python main.py`

    - Check the local host ('127.0.0.1') at port `8080`
    to see if the Flask app is working

* Create an `app.yaml` file to deploy the application in App 
Engine:

```
runtime: python38
```

### Set Up and Deploy the Flask API in the App Engine Service

* Go to GCP and create a Cloud Shell terminal

* Clone your repository by running `git clone` and `cd` to the 
repo directory

* Deploy the app in App Engine by running:

  - `gcloud app create`: to create a new service

  - `gcloud app deploy`: to deploy the service by following the 
  instructions given in the `app.yaml` file

* Select a region and check the services to deploy. Continue and 
wait for it to be deployed.

* Test the deployed app using the URL (Endpoint) provided by App 
Engine

## Set up Continuous Delivery using Cloud Build

* In your repository, create a `cloudbuild.yaml` file for CD:

```
steps:
- name: "gcr.io/cloud-builders/gcloud"
  args: ["app", "deploy"]
timeout: "1600s"
options:
  logging: CLOUD_LOGGING_ONLY
```

* In the Cloud Console search bar, search **Cloud Build**

* Go to Triggers > Create Trigger

  - Create trigger name and description

  - Event: Push to a branch

  - Source: Repository > Connect New Repository > GitHub and 
  authenticate and select repository

  - Source: Branch, select the branch to deploy

  - Service account: select the App Engine service account

  - Create

* Go to Settings > Service Account and enable App Engine and 
Service Account GCP services.

* Make a change to your repo and push to see the build in History
