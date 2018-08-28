# Google cloud commands on one place


* [General Project Settings](##general-project-settings)

  * [Set Project locally](##set-project-locally)
  
  * [Default Project](##default-project)
  
  * [Deploy Project](##deploy-project)
  
  * [Deploy Cron job](##deploy-cron-job)
  
* [Enable API Instructions](##enable-api-instructions)
  * [Permissions denied error](##permissions-denied)
* [Deploy React App](##deploy-react-app)
* [MySQL with Node.js](##mysql-with-node.js)

## General Project Settings

### Set Project locally
`gcloud config set project [project name]`


### Default Project
`gcloud config list --format 'value(core.project)' 2>/dev/null`


### Deploy Project
`gcloud app deploy [app].yaml --project=[project name]`


### Deploy Cron job
`gcloud app deploy cron.yaml --project=[project name]`


## Enable API Instructions

1. Go to - https://console.cloud.google.com/apis/dashboard?project=[project_name] and click to "ENABLE APIS AND SERVICES"
2. Create service account credentials - https://console.cloud.google.com/apis/credentials, "Create credentials" -> "Service account key" -> Download the json file -> Set your credentials in the console 

`export GOOGLE_APPLICATION_CREDENTIALS="[PATH]"` (for Linux or Mac) or 

`export GOOGLE_APPLICATION_CREDENTIALS="/home/user/Downloads/service-account-file.json"` (For Windows)



### Permissions denied
Important!
If you still see the error "Permissions denied" you might want to close and re-open your console.

## Deploy React App
* Cudus to [Majit](https://medium.com/tech-tajawal/deploying-react-app-to-google-app-engine-a6ea0d5af132) for the great tutorial

1. Run `npm run build` in your React app folder
2. Open Google cloud console -> Storage -> Create a bucket and upload your `build` folder
3. Create `app.yaml` file in your folder with the sample content and upload it as well:
```
runtime: python27
api_version: 1
threadsafe: true
handlers:
- url: /
  static_files: build/index.html
  upload: build/index.html
- url: /
  static_dir: build
```
4. Open Google cloud console and `mkdir new-app`
5. Run the command `gsutil rsync -r gs://[bucket name] ./new-app` and `glcoud app deploy`
6. You may need to wait for few minutes for the changes to take place

## MySQL with Node.js

1. Create new instance in gcloud console
2. Run the sql sever locally:
  - Mac/Linux: `./cloud_sql_proxy -instances="[YOUR_INSTANCE_CONNECTION_NAME]"=tcp:3306`
  - Windows: `cloud_sql_proxy.exe -instances="[YOUR_INSTANCE_CONNECTION_NAME]"=tcp:3306`
3. Follow the instructions [here](https://github.com/GoogleCloudPlatform/nodejs-getting-started/tree/master/2-structured-data) to build your nodejs server

Important!

You may see the error message "Cloud SQL Administration API has not been used in project XXX before or it is disabled."

Obviosly, you first have to enable the API, but if this doesn't do the trick, then you have to create your service account credentials (as described in Enable API Instructions section) and then set your credentials location with the following command:

`./cloud_sql_proxy -instances="[YOUR_INSTANCE_CONNECTION_NAME]"=tcp:3306  -credential_file="./service_account.json"`

Make sure to add `INSTANCE_CONNECTION_NAME` variable in your CONFIG file.

Also in app.yaml you should add:

beta_settings:
  cloud_sql_instances: [INSTANCE_CONNECTION_NAME]

## Add credentials in Dialogflow

from google.oauth2.service_account import Credentials
credentials = Credentials.from_service_account_file('<service_account_json>')
session_client = dialogflow_v2.SessionsClient(credentials=credentials)
