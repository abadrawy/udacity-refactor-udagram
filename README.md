# udacity-refactor-udagram

## what you need to start off
* An aws account
* docker installed and a dockerhub account
* nodejs and npm installed

## Divide the application into smaller services 

* Importing starter code https://github.com/scheeles/cloud-developer/tree/06-ci/course-03/exercises
* Setting up and exporting environemnt varlibles that are read in the docker-compose files such as AWS_BUCKET, AWS_PROFILE, AWS_REGION, JWT_SECRET, POSTGRESS_DATABASE, POSTGRESS_DB, POSTGRESS_HOST, POSTGRESS_PASSWORD, POSTGRESS_USERNAME & URL. 

## Containerize the application
* Editing the docker build files for the small services, to point to specific dockerHub username. 
* Running the services as docker images, building them and pushing them to the dockerHub directory.
  * Build the images: docker-compose -f docker-compose-build.yaml build --parallel
  * Push the images: docker-compose -f docker-compose-build.yaml push
  * Run the container: docker-compose up


## Create the Kubernetes resource and deploy it to a Kubernetes cluster
* Used brew for installation. 
* Followed this getting started guide for installing kops and connecting it to AWS, https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md
* After having this directory as our root udacity-c3-deployment/k8s, we edit the config files and make sure they point to our contianrer images in our dockerHub registeroy. 
* Add base64 encoded values in aws-secet.yaml, env-secret.yaml and decode the env-configmap files, before applying the deployments and services.
* Apply the following commands to lauch deployment nodes:
  * kubectl apply -f backend-feed-deployment.yaml
  * kubectl apply -f backend-user-deployment.yaml
  * kubectl apply -f reverseproxy-deployment.yaml
  * kubectl apply -f frontend-deployment.yaml
* Apply the following commanfs to lauch services:
  * kubectl apply -f backend-feed-service.yaml
  * kubectl apply -f backend-user-service.yaml
  * kubectl apply -f reverseproxy-service.yaml
  * kubectl apply -f frontend-service.yaml
* Use this command to view state of the pods
  * kubectl get pods
* Expose the application through the services, using the following commands:
  * kubectl port-forward service/reverseproxy 8080:8080 
  * kubectl port-forward service/frontend 8100:8100 

## Extend the application with deployments and be able to do rolling-updates and rollbacks
* Use the following commands for rolloing-updates
  * kubectl rollout status deployment reverseproxy
  * kubectl rollout status deployment backend-feed
  * kubectl rollout status deployment backend-user
  * kubectl rollout status deployment frontend
* Use the follwing commands to validate the updates
  * kubectl describe deployment reverseproxy
  * kubectl describe deployment frontend
  * kubectl describe deployment backend-user
  * kubectl describe deployment backend-feed
* Use this command to rollback, kubectl rollout undo deployment <name>
* Use this command to scale the user service, kubectl scale deployment/user --replicas=<instances>


## Monitoring using aws cloudWatch
* cloudWatch was configured following this link, https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-setup-metrics.html
*  A directory in the cloudWatch was created for the deployed cluster where the logs will be avalaible. 

## Automatic continuous integration and continuous delivery.
* Create a travis account using you gitub account.
* Link travis to the project repositroy.
* Add a .travis.yaml file in the root directory of the project. 
* Trigger a build from travis dashboard for the current project repo. 

