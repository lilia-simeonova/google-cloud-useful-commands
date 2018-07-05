# Google cloud commands on one place

## General Project Deployment

### Set Project locally
`gcloud config set project [project name]`


### Check which is your Default Project
`gcloud config list --format 'value(core.project)' 2>/dev/null`


### Deploy Project
`gcloud app deploy [app].yaml --project=[project name]`


### Deploy Cron job
`gcloud app deploy cron.yaml --project=[project name]`


Enable API Instructions

1. Go to - https://console.cloud.google.com/apis/dashboard?project=[project_name] and click to "ENABLE APIS AND SERVICES"
2. Create service account credentials - https://console.cloud.google.com/apis/credentials, "Create credentials" -> "Service account key" -> Download the json file -> Set your credentials in the console 

`export GOOGLE_APPLICATION_CREDENTIALS="[PATH]"` (for Linux or Mac) or 

`export GOOGLE_APPLICATION_CREDENTIALS="/home/user/Downloads/service-account-file.json"` (For Windows)

Important!

If you still see the error "Permissions denied" you might want to close and re-open your console.

## Deploy React App

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
5. Run the command `gsutil rsync -r gs://[bucket name] ./new-app`
