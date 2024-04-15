# Steps to Recreate
This sample is intended to be run on Google Cloud. You will need a Google Cloud project with access to create VPC Networks, Cloud Build Private Pools, and submit Cloud Build jobs.

## Configure Infrastructure
1. Create a VPC to use for your Cloud Build Private Worker Pool, no subnets necessary.
2. Create a Cloud Build Private Worker Pool using your VPC ([Service Producer](https://cloud.google.com/build/docs/private-pools/set-up-private-pool-to-use-in-vpc-network) network option).
    - Use the region 'asia-northeast1'
3) Repeat step 2) for 'asia-southeast1', and 'me-central1'
4) Create Artifact Registry Remote Repositories
    - Format: Maven
    - Mode: Remote
    - Remote Repository Source: Maven Central
    - Location Type: Region
    - Region: asia-northeast1
5) Repeast step 4) for 'asia-southeast1', and 'me-central1'

## Deployment
1. Replace pom.xml with the relevant POM for your test: pom-asia-northeast1.xml, pom-asia-southeast1.xml, or pom-me-central1.xml.
    - e.g. `cp pom-asia-northeast1.xml pom.xml`
2. Submit the Cloud Build Job for your chosen region.
    - e.g. `gcloud builds submit . --region=asia-northeast1 --config=cloudbuild-asia-ne1.yaml`
    - Note: ensure that the `config` and `region` are consistent with the region chosen to replate the `pom.xml`
3. Open the corresponding Artifact Registry Remote Repository and validate that the packages have all been cached.
4. Submit the Cloud Build Job again (to run with cached packages).

## Review the Results
1. View the results of the run with cached packages in their respective Cloud Build Regions:

| asia-northeast1  | asia-southeast1 | me-central1 |
| ------------- | ------------- | ------------- |
| ~50s  | ~35s  | ~230s |
