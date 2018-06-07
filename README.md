# Google cloud commands on one place

## General Project Deployment

### Set Project locally
gcloud config set project [project name]


### Check which is your Default Project
gcloud config list --format 'value(core.project)' 2>/dev/null


### Deploy Project
gcloud app deploy [app].yaml --project=[project name]


### Deploy Cron job
gcloud app deploy cron.yaml --project=[project name]


Enable API Instructions

Go to - https://console.cloud.google.com/apis/dashboard?project=[project name] and click to "ENABLE APIS AND SERVICES"
