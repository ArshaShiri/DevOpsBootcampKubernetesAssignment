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