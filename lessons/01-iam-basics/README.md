# Lesson 01 - IAM Basics

Follow along with the below commands to complete the exercise. Make sure to replace the BUCKET_NAME and PROJECT_ID placeholders with your desired bucket name and the ID of the project in which you are working.

First, let's make the bucket - make sure to give it a unique name as Google Cloud Storage bucket names must be globally unique:
`gcloud storage buckets create BUCKET_NAME --location=us-central1`

Then let's make a dummy file and place it in the bucket:
`echo "Hello world" > hello_world.txt`
`gcloud storage cp hello_world.txt gs://BUCKET_NAME/hello_world.txt`

Now let's create the service account we will be acting as for this exercise:

`gcloud iam service-accounts create bucket-service-account --description="A service account for demonstrating IAM permissions on GCS Buckets." --display-name="Bucket IAM Test"`

Now let's attempt to access the file:
`gcloud storage cat gs://BUCKET_NAME/hello_world.txt --impersonate-service-account=bucket-service-account@PROJECT_ID.iam.gserviceaccount.com`

You receive an error back:


Now let's try giving permissions:
`gcloud storage buckets add-iam-policy-binding BUCKET_NAME --member=serviceAccount:bucket-service-account@PROJECT_ID.iam.gserviceaccount.com --role="roles/storage.objectViewer"`

And try again:
`gcloud storage cat gs://BUCKET_NAME/hello_world.txt --impersonate-service-account=bucket-service-account@PROJECT_ID.iam.gserviceaccount.com`

>Hello world

Success!

Now, let's tidy up:
`gcloud storage buckets delete gs://BUCKET_NAME`
`gcloud iam service accounts delete bucket-service-account`
