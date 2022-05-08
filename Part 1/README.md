# Part 1

## Exercise 1.1
Output for docker ps -a
```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS     NAMES
d99956b77da9   nginx     "/docker-entrypoint.…"   26 seconds ago   Up 25 seconds               80/tcp    determined_liskov
e75f37ab9e1e   nginx     "/docker-entrypoint.…"   27 seconds ago   Exited (0) 5 seconds ago              relaxed_jones
3002b27ab38b   nginx     "/docker-entrypoint.…"   28 seconds ago   Exited (0) 14 seconds ago             magical_bell
```

## Exercise 1.2
Output for docker ps -a
```bash
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
Output for docker images
```bash
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

## Exercise 1.3
Secret message
```bash
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
```
Commands
```bash
docker run -it --name looper devopsdockeruh/simple-web-service
docker exec -it looper bash
/usr/src/app # tail -f ./text.log
```

## Exercise 1.4
Commands
```bash
docker run -d -it --name point ubuntu
docker exec -it point bash
/usr/src/app # apt-get install curl
/usr/src/app # exit
docker exec -it point sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
```
Output
```bash
Input website:
helsinki.fi
Searching..
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.helsinki.fi/">here</a>.</p>
</body></html>
```

## Exercise 1.5
Size of images
```bash
REPOSITORY                          TAG       IMAGE ID       CREATED         SIZE
devopsdockeruh/simple-web-service   ubuntu    4e3362e907d5   13 months ago   83MB
devopsdockeruh/simple-web-service   alpine    fd312adc88e0   13 months ago   15.7MB
```
Commands to test functionality
```bash
docker run -it --name looper devopsdockeruh/simple-web-service:alpine
docker exec -it looper sh
/usr/src/app # tail -f ./text.log
```
Output
```bash
2022-05-06 16:50:52 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2022-05-06 16:50:54 +0000 UTC
2022-05-06 16:50:56 +0000 UTC
...
```

## Exercise 1.6
Commands
```bash
docker run -it devopsdockeruh/pull_exercise
```
Secret message
```bash
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```

## Exercise 1.7
Commands
```bash
docker build . -t web-server
docker run web-server
```
Dockerfile
```bash
FROM devopsdockeruh/simple-web-service:alpine
CMD server
```

## Exercise 1.8
Dockerfile
```bash
FROM ubuntu:20.04
WORKDIR /usr/src/app
COPY search.sh .
RUN chmod +x search.sh
RUN apt update && apt install -y curl
CMD ./search.sh
```

## Exercise 1.9
Commands
```bash
touch text.log
docker run -d -v $(pwd)/text.log:/usr/src/app/text.log devopsdockeruh/simple-web-service
```

## Exercise 1.10
Commands
```bash
docker run -p 8080:8080 webserver
```

## Exercise 1.11
Dockerfile
```bash
FROM openjdk:8
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN ./mvnw package
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
```

## Exercise 1.12
Dockerfile
```bash
FROM ubuntu
EXPOSE 5000
WORKDIR /usr/src/app
RUN apt-get update && apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash
RUN apt-get install -y nodejs
COPY . .
RUN npm install
RUN npm run build
RUN npm install -g serve
CMD ["serve", "-s", "-l", "5000","build"]
```

## Exercise 1.13
Commands
```bash
docker build . -t example-backend
docker run -p 8080:8080 example-backend
```
Dockerfile
```bash
FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN go build
RUN go test ./...
CMD ["./server"]
```

## Exercise 1.14
Commands
```bash
docker build . -t example-backend
docker run -p 8080:8080 example-backend
docker build . -t example-frontend
docker run -p 5000:5000 example-frontend
```
Dockerfile example-frontend
```bash
FROM ubuntu
EXPOSE 5000
WORKDIR /usr/src/app
RUN apt-get update && apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash
RUN apt-get install -y nodejs
COPY . .
RUN npm install
ENV REACT_APP_BACKEND_URL=http://localhost:8080/ping 
RUN npm run build
RUN npm install -g serve
CMD ["serve", "-s", "-l", "5000","build"]
```
Dockerfile example-backend
```bash
FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN go build
RUN go test ./...
ENV REQUEST_ORIGIN=http://localhost:5000
CMD ["./server"]
```

## Exercise 1.15
This is the spring-example-project from Exercise 1.11.
Run the project using the following commands:
```bash
docker pull andytrieu/exercise1.15
docker run -p 8080:8080 andytrieu/exercise1.15
```
[Link to Docker Hub repository page](https://hub.docker.com/repository/docker/andytrieu/exercise1.15)

## Exercise 1.16

[Link to released application](https://exercise-1-16-docker.herokuapp.com/)
