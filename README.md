# lt-LocationTracker
This application contains a set of microservices and can be run in 3 ways as explained below:


## 1. DOCKER COMPOSE: This contains the docker-compose.yaml to run all the microservices..
Go to the directory with yaml file and  > docker-compose up

## 2. INDIVIDUAL DOCKER CONTAINERS: If you want to run DOCKERS locally (WITHOUT using docker-compose.yaml ):

```
docker run -d -p 8161:8161 -p 61616:61616 --name myqueue --network locationtracker mooneshkachroo/lt-activemq:0.0.1-RELEASE

docker run -d --network locationtracker --env spring.activemq.broker-url=tcp://myqueue:61616 --env fleetman.position.queue=positionQueue mooneshkachroo/lt-position-simulator:0.0.1-RELEASE

docker run -d -p 27017:27017 --name mongodb --network locationtracker mongo:3.6.5-jessie

docker run -d -p 8090:8090 --name positiontracker --network locationtracker --env spring.activemq.broker-url=tcp://myqueue:61616 --env fleetman.position.queue=positionQueue --env spring.data.mongodb.host=mongodb mooneshkachroo/lt-position-tracker:0.0.1-RELEASE

docker run -d -p 8080:8080 --name apigateway --network locationtracker --env position-tracker-url=http://positiontracker:8090 mooneshkachroo/lt-api-gateway:0.0.1-RELEASE

docker run -d --network locationtracker -p 80:80 --env SPRING_PROFILES_ACTIVE=localhost mooneshkachroo/lt-webapp:0.0.1-RELEASE
```
## 3. AWS WITH KUBERNETES : If you want to run the application microservices on AWS using K8S:

1. Create cluster in AWS using : https://docs.google.com/document/d/1fb3k7rgK9HFWktL-R43pwTbXQUGxsZi7pmHA0By-quQ/edit#
2. The folder yaml-files contains all the yaml documents requiered to build pods/servcies.
3. Ignore test.yaml (that was only for testing a piece of microservices)



