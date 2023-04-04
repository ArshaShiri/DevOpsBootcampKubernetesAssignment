# This project is for the Devops bootcamp exercise for "Container Orchestration with Kubernetes" 

## EXERCISE 1: Create a Kubernetes cluster

* Create a Kubernetes cluster (Minikube or LKE)

**Solution**

    minikube start

    # For LKE, Linode can be used

## EXERCISE 2: Deploy Mysql with 3 replicas
First of all, you want to deploy the mysql database.

* Deploy Mysql database with 3 replicas and volumes for data persistence

To simplify the process you can use Helm for that.

**Solution**

We can use the chart presented [here](https://github.com/bitnami/charts/tree/main/bitnami/mysql) to install using helm.

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm install my-release bitnami/mysql 

    # The running pods can be checked
    kubectl get pod


## EXERCISE 3: Deploy your Java Application with 3 replicas

Now you want to

* deploy your Java application with 3 replicas.

With docker-compose, you were setting env_vars on server. In K8s there are own components for that, so

* create ConfigMap and Secret with the values and reference them in the application deployment config file.


**Solution**

First we create a secrete for the db `db-secret.yaml`. We can then apply it:

    kubectl apply -f db-secret.yaml

We also set the config map file for the database and apply it:

    kubectl apply -f db-config.yaml