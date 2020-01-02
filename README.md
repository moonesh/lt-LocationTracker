# lt-LocationTracker
##This contains the docker-compose.yaml to run all the microservices

##RUN IN SEQUENCE for RUNNING DOCKERS LOCALLY (WITHOUT YAML):
1. docker run -d -p 8161:8161 -p 61616:61616 --name myqueue --network locationtracker mooneshkachroo/lt-activemq:0.0.1-RELEASE

2. docker run -d --network locationtracker --env spring.activemq.broker-url=tcp://myqueue:61616 --env fleetman.position.queue=positionQueue mooneshkachroo/lt-position-simulator:0.0.1-RELEASE

3. docker run -d -p 27017:27017 --name mongodb --network locationtracker mongo:3.6.5-jessie

4. docker run -d -p 8090:8090 --name positiontracker --network locationtracker --env spring.activemq.broker-url=tcp://myqueue:61616 --env fleetman.position.queue=positionQueue --env spring.data.mongodb.host=mongodb mooneshkachroo/lt-position-tracker:0.0.1-RELEASE

5. docker run -d -p 8080:8080 --name apigateway --network locationtracker --env position-tracker-url=http://positiontracker:8090 mooneshkachroo/lt-api-gateway:0.0.1-RELEASE

6. docker run --network locationtracker -p 80:80 --env SPRING_PROFILES_ACTIVE=localhost mooneshkachroo/lt-webapp:0.0.1-RELEASE


