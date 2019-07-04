# Idan Mishor DevOps Homework Assignment

## Pre-requisites
Before you start please make sure you have the following:

- GCP Account
- GCP user credentials
- Terraform installed
- Kubectl installed
- Google SDK Installed

## Delivery Objects
- Docker files for each application
- Terraform project to initialze GKE cluster
- CI/CD proccess

# Default Configurations:
 - The default name for the GKEcluster is **playground**
 - Default region for cluster deployment is **europe-west1**
 - Default deployment name is **hw-app**
 - The default name for app_b service is **internal-db-service**
   To change the default name please follow the below steps:
    - **note that the service name can't have underscore (_) in it since it needs to be DNS resolvable**
    - export an environment variable named "db_service_name" with the new service name
    
       `export db_service_name = "new-db-service"`
    - build new version on app_a with an argument in the docker build command. 
    
       `docker build -t app_a --build-arg db_service_tag=${db_service_name}`
    - upload the containers to the container registry
    
# Run-Book for deployment
In order to provision the environment please use the following steps:

 - clone the repo localy
 - create the GKE cluster:
    - go into the terraform folder and type `terraform init`
    - run `terraform plan`
    - run `terraform apply` to create the cluster
after few minutes your cluster will be ready
 - get authentication credentials to interact with the cluster:
 
   `gcloud container clusters get-credentials playground --region europe-west1`
 - deploy the deployments and services for each app
 ```
    kubectl apply -f web-deployment.yaml
    kubectl apply -f web-service.yaml
    kubectl apply -f db-deployment.yaml
    kubectl apply -f db-service.yaml
 ```

# Autoscale handeling
- **The terraform project takes care of autoscaling the cluster as it sets minimum and maximun nodes per zone**
- enable autoscale on each service by running the bellow commands:
```
  kubectl autoscale deployment {{ app_a_deployment name}} --max 10 --min 2 --cpu-percent 50
  kubectl autoscale deployment {{ app_b deployment name }} --max 10 --min 2 --cpu-percent 50
```
