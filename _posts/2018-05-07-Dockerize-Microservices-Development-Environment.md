---
layout: post
title: Dockerize your Microservices Development Environment
categories: Docker, Microservices, Spring Boot, NGINX
---

I have worked on several microservices-based applications on the cloud, and had to go through relatively slow CI process to get to see the changes and test them. Also, writing things from scratch is kind of my hobby. So, I thought I will create a small demo setup and will keep adding to it as a pet project.

## Demo Application Components
The application is composed of two Spring Boot services providing the notifications and messages stored in a MongoDB database with frontend developed in Angular 5 and making use of NGINX to proxy calls to the corresponding service.
![Demo Structure](/assets/docker-microservices-diagram.png)

### Directory Structure
![Directory Structure](/assets/docker-microservices-dev-env-directory-structure.png)

### Docker Files and Configurations
I will go through the configuration of each component of the demo application, highlighting the less obvious parts.

#### MongoDB
MongoDB container is pretty straightforward, just using the mongo image and exposing the right port.
```
FROM mongo
EXPOSE 27017
```

#### Spring Boot Services
The Spring Boot services are simple to containerize, just pull OpenJDK 8 and run Java runtime against the jar file of the service.
```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG JAR_FILE=<Spring-Boot Service Jar File>.jar
COPY ./target/${JAR_FILE} app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app.jar"]
```
It is important to note that all services will be exposing port 8080, and addressed using their host name defined in docker-compose.
Also, for configuring MongoDB client in the Spring Boot services, just address it with the host name, "DB" in this context.
```
spring:
  data:
    mongodb:
      uri: mongodb://db:27017/<DB Name>
```

#### NGINX
Nginx is a bit tricky to configure and containerize. In the demo application Nginx has two roles; one to serve the Angular application and the other to proxy the API calls to the corresponding backend services, so that the Angular application doesn't have to worry about locating the services by their URL. All calls from the Angular app goes to /api/<service name> and Nginx proxies the call to the right upstream.
The dockerization of Nginx copies the configuration from the local repository and copies the built Angular application and starts the server.
```
FROM nginx
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./frontend/frontend-app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Nginx configuration in simple words are stating that the server is running on port 80, and states the root folder for the Angular app, then configures each service as an upstream that will be used in the proxy configuration based on the URL.
```
http {
  resolver  127.0.0.11;   # Docker DNS Internal resolver
  server {
    listen    80;

    location / {
      root    /usr/share/nginx/html;
      index   index.html;
    }
    location /api/messages {
      proxy_pass http://api-messaging/api/messages;
    }
    location /api/notifications {
      proxy_pass http://api-notifications/api/notifications;
    }
  }

  upstream api-messaging {
    server messaging-service:8080;
  }

  upstream api-notifications {
    server notifications-service:8080;
  }
}

events {
  worker_connections 1024;
}
```
In a later article I will use the same configuration and assign a new role to Nginx to do load balancing for a swarm of services.

#### Docker Compose
The docker compose mainly glues all parts together in a private network and only expose the Nginx HTTP port 80 to the outside world.
Note that all services attached to the same network, otherwise the system will fail to communicate internally.
```
version: '3'
services:
  db:
    build:
      context: .
      dockerfile: mongo.dockerfile
    expose:
      - "27017"
    networks:
      - app-network

  messaging-service:
    restart: always
    container_name: messaging-service
    build:
      context: ./backend/messaging-service
      dockerfile: Dockerfile
    expose:
      - "8080"
    depends_on:
      - db
    networks:
      - app-network

  notifications-service:
    restart: always
    container_name: notifications-service
    build:
      context: ./backend/notifications-service
      dockerfile: Dockerfile
    expose:
      - "8080"
    depends_on:
      - db
    networks:
      - app-network

  nginx:
    restart: always
    build:
      context: .
      dockerfile: nginx.dockerfile
    expose:
      - "80"
    ports:
      - "80:80"
    depends_on:
      - messaging-service
      - notifications-service
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

### Conclusion
Docker and containerization allows developers to simulate the production environment with multiple distributed components communicating to the provide the needed functionality without having too many server applications or many databases for the different projects or different application setups cluttering the main developer machine. Personally I have been using Docker and Vagrant to have sandboxed environments as much as I can since they started to see the light.
If you would like to take a look at the full demo application code, you can check it out on GitHub
[Demo Application ](https://github.com/ahmedabadawi/playground-docker-spring-angular), however, this is the main playground code base, it can vary from the details in this article, to download the tag with just the details in this article, follow one of these links [.zip](https://github.com/ahmedabadawi/playground-docker-spring-angular/archive/v1.0.zip) [.tar.gz](https://github.com/ahmedabadawi/playground-docker-spring-angular/archive/v1.0.tar.gz).
Finally, I hope this article was helpful to you and I would love your feedback and comments on it.

Happy Coding...
