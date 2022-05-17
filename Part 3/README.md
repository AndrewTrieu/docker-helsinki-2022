# Part 3

## Exercise 3.1
The course page was deployed to Heroku via GitHub Actions.<br />
.github/workflows/build.yml
```yml
name: Build and Deploy

on:
  push:
    branches:
      - master
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/devopspage:latest
      - name: Deploy on heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
          usedocker: true
```

## Exercise 3.2
Skipped.

## Exercise 3.3
Dockerfile of frontend
```Dockerfile
FROM ubuntu:18.04
EXPOSE 5000
WORKDIR /usr/src/app
ENV REACT_APP_BACKEND_URL=http://localhost:8080/ping 
COPY . .
RUN apt-get update && apt-get install -y curl 
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash
RUN apt-get install -y nodejs && apt-get purge -y --auto-remove curl 
RUN npm install 
RUN npm run build 
RUN npm install -g serve 
RUN useradd -m appuser 
RUN chown appuser .
USER appuser
CMD ["serve", "-s", "-l", "5000","build"]
```
Dockerfile of backend
```Dockerfile
FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
ENV REQUEST_ORIGIN=http://localhost
COPY . .
RUN go build
RUN go test ./...
RUN useradd -m appuser
RUN chown appuser .
USER appuser
CMD ["./server"]
```

## Exercise 3.4
|             |     Frontend     |     Backend    |
|-------------| ---------------- | -------------- |
|   Before    |     585.24MB     |     1.07GB     |
|    After    |     565.26MB     |     1.07GB     |

## Exercise 3.5
|             |     Frontend     |      Backend     |
|-------------| ---------------- | ---------------- |
|   Before    |     565.26MB     |      1.07GB      |
|    After    |     376.13MB     |     482.43MB     |

## Exercise 3.6
|             |     Frontend     |      Backend     |
|-------------| ---------------- | ---------------- |
|   Before    |     376.13MB     |     482.43MB     |
|    After    |     71.12MB      |      23.41MB     |



## Exercise 3.7
This is taken from exercise 1.11.
|             |     spring-example-project    |
|-------------| ----------------------------- | 
|   Before    |           611.35MB            | 
|    After    |           120.82MB            | 

## Exercise 3.8

![Exercise 3 8](https://user-images.githubusercontent.com/68151686/168853751-6ce9f46c-5ffe-4dc7-b521-201a894d54f7.png)
