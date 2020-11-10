# iss-api-gateway

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=iss-api-gateway&metric=alert_status)](https://sonarcloud.io/dashboard?id=iss-api-gateway)

We decided to implement it in an extremely simplified version of an insurance sales system to test the following aspects of microservice development:

* Project creation and development
* Access of both relational and NoSQL databases
* Blocking and non-blocking operations implementation
* Microservice to microservice communication (synchronous and asynchronous)
* Service discovery
* Running background jobs

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