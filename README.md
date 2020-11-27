# iss-api-gateway

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=iss-api-gateway&metric=alert_status)](https://sonarcloud.io/dashboard?id=iss-api-gateway)

We decided to implement it in an extremely simplified version of an insurance sales system to test the following aspects of microservice development:

* Project creation and development
* Access of both relational and NoSQL databases
* Blocking and non-blocking operations implementation
* Microservice to microservice communication (synchronous and asynchronous)
* Service discovery
* Running background jobs

![Architecture](https://github.com/MasterCloudApps-Projects/iss-api-gateway/blob/master/images/iss-architecture.jpg?raw=true)

The complexity of “business microservices” was hidden by using a Gateway pattern. This component was responsible for the proper redirection of requests to the appropriate services based on the configuration. The frontend application could only communicate with this component. This component showed the usage of non-blocking http declarative clients.

## How to run Application

Running application from command line using Docker, this is the cleanest way.
In order for this approach to work, of course, you need to have Docker installed in your local environment.

* From the root directory you can run the following command:<br/>
    ```docker-compose -f docker/docker-compose.yml up --build```
* Application will be running on: http://localhost:8084
* To stop it you can open other terminal in the same directory, and then run the following command:<br/>
    ```docker-compose -f docker/docker-compose.yml down```

## How to run the Unit Test

```mvn -B clean verify```

## Deploy solution in minikube

# Prerequisites

- Minikube cluster with kubectl installed and configured to use your cluster

1. Run minikube
```
minikube start --memory 8192
```
2. Create a ClusterRoleBinding for default namespace
```
kubectl create clusterrolebinding admin --clusterrole=cluster-admin --serviceaccount=default:default
```
3. Create a config map of Postgres
```
kubectl apply -f k8s/postgres/postgres-config.yaml
```
4. Deploy Postgres with a persistent volume claim
```
kubectl apply -f k8s/postgres/volume.yaml
kubectl apply -f k8s/postgres/postgres.yaml
```
5. Create a config map and secret of Mongodb
```
kubectl apply -f k8s/mongodb/mongodb-config.yaml
kubectl apply -f k8s/mongodb/mongodb-secret.yaml
```
6. Deploy Mongodb with a persistent volume claim
```
kubectl apply -f k8s/mongodb/volume.yaml
kubectl apply -f k8s/mongodb/mongodb.yaml
```
7. Create a config map of Kafka service
```
kubectl apply -f k8s/kafka/kafka-config.yaml
```
8. Deploy Zookeeper and Kafka Cluster
```
kubectl apply -f k8s/kafka/zookeeper-services.yaml
kubectl apply -f k8s/kafka/zookeeper-cluster.yaml
kubectl apply -f k8s/kafka/kafka-service.yaml
kubectl apply -f k8s/kafka/kafka-cluster.yaml
```
9. Deploy Pricing service
```
kubectl apply -f k8s/iss-pricing-service-deployment.yaml
```
10. Deploy Product service
```
kubectl apply -f k8s/iss-product-service-deployment.yaml
```
11. Deploy Policy service
```
kubectl apply -f k8s/iss-policy-service-deployment.yaml
```
12. Deploy Policy search service
```
kubectl apply -f k8s/iss-policy-search-service-deployment.yaml
```
13. Deploy Payment service
```
kubectl apply -f k8s/iss-payment-service-deployment.yaml
```
14. Deploy Api gateway service
```
kubectl apply -f k8s/iss-api-gateway-deployment.yaml
```
15. Active ingress controller plugin
```
minikube addons enable ingress
```
16. Deploy ingress
```
kubectl apply -f k8s/ingress.yaml
```
17. Define mastercloudapps.com local DNS
```
export MINIKUBE_IP=$(minikube ip)
echo $MINIKUBE_IP mastercloudapps.com | sudo tee --append /etc/hosts >/dev/null
```
18. Check status pods and services with kubectl get pods,services
```
kubectl get pods,services
```
![Pods](https://github.com/MasterCloudApps-Projects/iss-api-gateway/blob/master/images/pods-services.png?raw=true)

19. run dashboard and check status services in dashboard
```
minikube dashboard
```

![Dashboard](https://github.com/MasterCloudApps-Projects/iss-api-gateway/blob/master/images/services-dashboard.png?raw=true)


20. an example of endpoint execution

![Mastercloudapps](https://github.com/MasterCloudApps-Projects/iss-api-gateway/blob/master/images/mastercloudapps-example.png?raw=true)


21. Deleting all the Resources

```
kubectl delete -f k8s/ingress.yaml
kubectl delete -f k8s/iss-api-gateway-deployment.yaml
kubectl delete -f k8s/iss-payment-service-deployment.yaml
kubectl delete -f k8s/iss-policy-search-service-deployment.yaml
kubectl delete -f k8s/iss-policy-service-deployment.yaml
kubectl delete -f k8s/iss-product-service-deployment.yaml
kubectl delete -f k8s/iss-pricing-service-deployment.yaml
kubectl delete -f k8s/kafka/kafka-config.yaml
kubectl delete -f k8s/kafka/kafka-cluster.yaml
kubectl delete -f k8s/kafka/kafka-service.yaml
kubectl delete -f k8s/kafka/zookeeper-cluster.yaml
kubectl delete -f k8s/kafka/zookeeper-services.yaml
kubectl delete -f k8s/mongodb/mongodb.yaml
kubectl delete -f k8s/mongodb/volume.yaml
kubectl delete -f k8s/mongodb/mongodb-config.yaml
kubectl delete -f k8s/mongodb/mongodb-secret.yaml
kubectl delete -f k8s/postgres/postgres.yaml
kubectl delete -f k8s/postgres/volume.yaml
kubectl delete -f k8s/postgres/postgres-config.yaml
```

22. Stop minikube
```
minikue stop
```