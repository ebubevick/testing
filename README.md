# Deploying a Voting App Using Kubernetes

This guide will walk you through deploying a voting app with five microservices using Kubernetes from Docker samples. The app consists of a web vote app (Python), Redis (message queue), a worker service (.NET), a database (PostgreSQL), and a web result app (Node.js). We will write Kubernetes deployment YAML files for each microservice, manage their deployments, expose the necessary services to the internet, and scale the deployment as needed.

This is based on the original example-voting-app repository from the docker-examples GitHub page and modified it to work on the Kubernetes cluster.

## Prerequisites

Before proceeding, ensure you have the following installed:

- [Docker](https://docs.docker.com/get-docker/)
- [Kubernetes cluster](https://kubernetes.io/docs/setup/) (e.g., Minikube, Docker Desktop with Kubernetes enabled)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Step 1: Dockerizing the Microservices

Dockerize each microservice following Docker best practices. This typically involves creating a Dockerfile for each component, building Docker images, and pushing them to a container registry.

## Step 2: Writing Kubernetes Deployment YAML Files

Create or download Kubernetes Deployment YAML files the git repository for each microservice. These files describe the desired state for each microservice and instruct Kubernetes on how to manage their containers.

![Alt text](deploy_and_services.png)

# YAML Format for Deployment and Service

![Alt text](deployment_services_format.png)
what the deployments and services should look like.

## Step 3: Running the Deployments

Create deployments for each microservice using

`kubectl create -f <filename.yaml>` for each YAML file.

```bash
kubectl create -f voting-app-deploy.yaml
kubectl create -f redis-deploy.yaml
kubectl create -f worker-app-deploy.yaml
kubectl create -f postgres-deploy.yaml
kubectl create -f result-app-deploy.yaml
```

![Alt text](voting-app-deploy-service-created.png)

![Alt text](redis-deploy-service-created.png)

![Alt text](worker-deploy-created-running.png)

![Alt text](postgres-deploy-service-created.png)


## Step 4: Exposing the Services

Expose the necessary services to the internet using Kubernetes Services. You may need to expose services like the web vote app and web result app, while keeping others internal.

# PostgreSQL Yaml File

![Alt text](postgres-db-security.png)

**Caution:** The provided PostgreSQL deployment YAML file uses default credentials (`postgres`/`postgres`) and has security implications. This is suitable for testing and development purposes but is not recommended for production deployments. For production environments, ensure you follow best practices for securing your PostgreSQL deployment. If you need guidance on securing your PostgreSQL deployment for production, feel free to reach out to me on LinkedIn.

# Apply Services for exposing the web voting-app

Create the Service YAML files:

kubectl create -f voting-app-service.yaml
kubectl create -f redis-service.yaml
kubectl create -f postgres-service.yaml
kubectl create -f result-app-service.yaml

![Alt text](voting-app-deploy-service-running.png)
![Alt text](redis-deploy-service-running.png))
![Alt text](postgres-deploy-service-running.png)
![Alt text](result-app-deploy-service-created-running.png)


# Step 5: Accessing the Voting App

Use Minikube's service command to get the URL for accessing the voting app and result app.

#```bash
minikube service <service-name> --url

Replace <service-name> with the name of the service you want to access (e.g., voting-app, result-app). This command will provide you with the URL to access the specified service.

![Alt text](url-votingApp-resultApp.png)


Access the voting app and result app using the provided URLs.

![Alt text](voting-app-page.png)
![Alt text](result-app-page.png)



# Step 6: Scaling the Deployment

To scale the deployment, use the `kubectl scale` command with the `--replicas` flag.

```bash
kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>
```

Replace <deployment-name> with the name of the deployment you want to scale and <number-of-replicas> with the desired number of replicas.

**For example, to scale from 1 to 7 replicas for the voting-app-deployment:**

```bash
kubectl scale deployment voting-app --replicas=7
```
![Alt text](deploy-scale-incr-7.png)

![Alt text](deploy-newly-added-pods.png)
The newly created pods

**And to scale from 7 to 2 replicas:**
```bash
kubectl scale deployment voting-app --replicas=2
```
![Alt text](deploy-decre-2-terminating.png)
Terminating

![Alt text](deploy-decresed-2.png)
Running 2 Pod's

**And to scale from 2 to 3 replicas:**

kubectl scale deployment voting-app --replicas=3

This command adjusts the number of replicas running for the specified deployment, allowing you to dynamically scale your application based on demand.

## Observing Multiple Pods Being Used to Vote

As you vote from different browser windows/tabs, Kubernetes will distribute the requests among the available pods of the voting app deployment. You can verify this by checking the pod logs or using Kubernetes Dashboard to monitor pod activity.

By following these steps, you can demonstrate the functionality of the voting app, view the voting results, and observe multiple pods being utilized for voting simultaneously.

![Alt text](deploy-pod1-wepage.png)
voting using pod voting-app-deploy-96f58547f-5mhl7

![Alt text](deploy-pod2-wepage.png)
voting using pod voting-app-deploy-96f58547f-ptwpl

![Alt text](2votes-2browser-each-vote-registered-on-a-diff-pod.png)
voting from two seperate browser and seein the votes







