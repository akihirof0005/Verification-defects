# issue
Case: If you create the following docker-compose.yml
```
version: "3"
services:
  rest-client:
    container_name: curl
    image: tutum/curl
    command: sleep infinity

  web1:
    container_name: api1
    build: ./api1
    command: ruby main.rb -o 0.0.0.0

  api1:
    container_name: api2
    build: ./api2
    command: ruby main.rb -o 0.0.0.0

networks:
  default:
    name: net1
```
Note that "api1" can be used for both service and container names.

In this case, name resolution will not be possible within the "net1" network.
Specifically, the container name cannot be used for name resolution, and other service names are connected to the same container. randomly.

## Verification
```
$ git clone https://github.com/akihirof0005/Verification-defects.git
$ cd Verification-defects
$ docker-compose up -d
$ docker exec -it curl bash
# watch -n 2 curl "http://api1:4567/"
```
api1 and api2 are running on ruby, but this is irrelevant.
In fact, when this first happened, it happened in Java.

