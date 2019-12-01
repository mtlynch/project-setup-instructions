# appengine-deploy-instructions

I'm writing this because I always forget the little gotchas for creating and deploying a new AppEngine app.

## Setting up a new Go App Engine Standard project

1. Create new GCP project.
1. [Create a service account](https://console.cloud.google.com/iam-admin/serviceaccounts/create) specifically for deploying from CI:
  1. Give it the following roles:
      * App Engine Admin
      * Cloud Build Editor
      * Storage Admin
  1. Download service account key as JSON.
1. base64 encode JSON key: `cat service-account-creds.json | base64 --wrap=0 && echo ""`
1. Save the base64 encoded string as a CircleCI environment variable `CLIENT_SECRET`
1. [Enable AppEngine API](https://console.developers.google.com/apis/api/appengine.googleapis.com/overview)
1. Copy deployment config from [What Got Done](https://github.com/mtlynch/whatgotdone/blob/2fee6628d1057c47b27ce521fc7256ef29854358/.circleci/config.yml#L84-L114).
  1. Change `GCLOUD_PROJECT` to the gcloud project ID for your project (not the project *name*).

## Setting up a new Firebase hosting project

1. Go to [Firebase console](https://console.firebase.google.com/).
1. Click "Add Project" and create the project.
1. Copy `.firebaserc` and `firebase.json` from an existing project (e.g., [zestful-frontend](https://github.com/mtlynch/zestful-frontend)).
  * Customize the project IDs and rules.
1. Generate a firebase deploy token: `firebase login:ci`
1. Save the deploy token as a Circle CI environment variable: `FIREBASE_DEPLOY_TOKEN`
1. Copy deployment config from hello-world-vue-static:
  1. [Persist the necessary files](https://github.com/mtlynch/hello-world-vue-static/blob/5d13fcf35a53328c9078a867dbce9a96cc927598/.circleci/config.yml#L14-L19) to the workspace.
  1. [Deploy them to firebase](https://github.com/mtlynch/hello-world-vue-static/blob/5d13fcf35a53328c9078a867dbce9a96cc927598/.circleci/config.yml#L20-L32).