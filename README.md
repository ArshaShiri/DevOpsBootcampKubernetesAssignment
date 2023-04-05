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
    helm install my-release bitnami/mysql -f mysql-chart-values.yaml

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

Finally, we need to pull the image of this application from a docker repository. In this case, a personal repository is utilized.

    # We have to set the username and password that is going to be used for docker access:
    DOCKER_REGISTRY_SERVER=docker.io
    DOCKER_USER="user-id"
    DOCKER_EMAIL="user-email-for-docker-login"
    DOCKER_PASSWORD="password"

    kubectl create secret docker-registry my-registry-key \
    --docker-server=$DOCKER_REGISTRY_SERVER \
    --docker-username=$DOCKER_USER \
    --docker-password=$DOCKER_PASSWORD \
    --docker-email=$DOCKER_EMAIL

    # We can finally apply the java app
    kubectl apply -f java-app.yaml

    # At this point: 
    kubectl get pod
        # NAME                                   READY   STATUS    RESTARTS   AGE
        # java-app-deployment-64d59f6f77-n4mx5   1/1     Running   0          3m27s
        # java-app-deployment-64d59f6f77-z25l9   1/1     Running   0          3m25s
        # java-app-deployment-64d59f6f77-z9cxr   1/1     Running   0          3m32s
        # my-release-mysql-primary-0             1/1     Running   0          6m12s
        # my-release-mysql-secondary-0           1/1     Running   0          6m12s
        # my-release-mysql-secondary-1           1/1     Running   0          5m6s


## EXERCISE 4: Deploy phpmyadmin
As a next step you

* deploy phpmyadmin to access Mysql UI.

For this deployment you just need 1 replica, since this is only for your own use, so it doesn't have to be High Availability. A simple deployment.yaml file and internal service will be enough.