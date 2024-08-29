# Continuous Delivery Structure for a Flask App using GCP App Engine

## Creating and Deploying a Flask App in App Engine

Process steps:

1. Create a project repository in GitHub and add the structure files:

    * `Makefile`: to build the application

    ```
    install:
	    pip install -r requirements.txt
    ```

    * `requirements.txt`: file with the required dependencies

    ```
    flask
    ```

    * `main.py`: simple Flask app in Python

2. Create and activate a virtual environment:

    * `virtualenv ~/.flask-cd`

    * `source ~/.flask-cd/bin/activate`

3. Install dependencies: `make install`

4. Test the Flask app by running `python main.py`

    * Check the local host ('127.0.0.1') at port `8080`
    to see the Flask app is working

5. Create an `app.yaml` file to deploy the application in App Engine:

```
runtime: python38
```

6. Go to GCP and create a Cloud Shell terminal. `git clone` your GitHub
repository. Go to the repository directory

7. Deploy app in App Engine by running `gcloud app deploy`. Select a region
and check the services to deploy. Continue and wait for it to be deployed.

8. Test the deployed app in the url provided.

## Setting up Continuous Delivery using Cloud Build

1. In your repository, create a `cloudbuild.yaml` file for CD:

```
steps:
- name: "gcr.io/cloud-builders/gcloud"
  args: ["app", "deploy"]
timeout: "1600s"
options:
  logging: CLOUD_LOGGING_ONLY
```

2. In the Cloud Console search bar, search **Cloud Build**

3. Go to Triggers > Create Trigger

    * Create trigger name and description

    * Event: Push to a branch

    * Source: Repository > Connect New Repository > GitHub and authenticate
    and select repository

    * Source: Branch, select the branch to deploy

    * Service account: select the App Engine service account

    * Create

4. Go to Settings > Service Account and enable App Engine and Service Account
GCP services.

5. Make a change to your repo and push to see the build in History