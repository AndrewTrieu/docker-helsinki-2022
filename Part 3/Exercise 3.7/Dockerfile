FROM openjdk:8 as build-stage
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN ./mvnw package
FROM openjdk:alpine
WORKDIR /root/
COPY --from=build-stage /usr/src/app//target/docker-example-1.1.3.jar .//target/docker-example-1.1.3.jar
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
