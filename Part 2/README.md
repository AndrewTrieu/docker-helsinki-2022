# Part 2

## Exercise 2.1
docker-compose.yml
```yml
version: '2.5'

services:
  simple-web-service:
    image: devopsdockeruh/simple-web-service
    volumes:
      - ./text.log:/usr/src/app/text.log
```

## Exercise 2.2
docker-compose.yml
```yml 
version: '2.5'

services:
  simple-web-service:
    image: devopsdockeruh/simple-web-service
    ports:
      - 8080:8080
    command: server
    volumes:
      - ./text.log:/usr/src/app/text.log
```

## Exercise 2.3
docker-compose.yml
```yml
version: '2.5'

services:
  frontend:
    image: frontend
    build: ./material-applications/example-frontend
    ports:
      - 5000:5000
    container_name: frontend
  backend:
    image: backend
    build: ./material-applications/example-backend
    ports:
      - 8080:8080
    container_name: backend
```

## Exercise 2.4
docker-compose.yml
```yml
version: '2.5'

services:
  redis:
    image: redis
  frontend:
    image: frontend
    ports:
      - 5000:5000

  backend:
    image: backend
    ports:
      - 8080:8080
    container_name: backend
    environment:
      - REDIS_HOST=redis
```

## Exercise 2.5
Commands
```bash
docker-compose up -d --scale compute=5
docker-compose ps
```

## Exercise 2.6
docker-compose.yml
```yml
version: '2.5'

services:
  postgres:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: postgres
    volumes:
      - database:/var/lib/postgresql/data
  redis:
    image: redis
  frontend:
    image: frontend
    ports:
      - 5000:5000
    container_name: frontend
  backend:
    image: backend
    ports:
      - 8080:8080
    container_name: backend
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
      - POSTGRES_PASSWORD=postgres
      
volumes:
  database:
```

## Exercise 2.7
docker-compose.yml
```yml
version: '2.5'

services:
  frontend:
    build: ./ml-kurkkumopo-frontend
    ports:
      - 3000:3000
  backend:
    build: ./ml-kurkkumopo-backend
    ports:
      - 5000:5000
    volumes:
      - ./model:/src/model
  training:
    build: ./ml-kurkkumopo-training
    volumes:
      - ./imgs:/src/imgs
      - ./model:/src/model

volumes:
  database:
```

## Exercise 2.8
docker-compose.yml
```yml
version: '2.5'

services:
  nginx:
    image: nginx:1.19-alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - 80:80
  postgres:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: postgres
    volumes:
      - database:/var/lib/postgresql/data
  redis:
    image: redis
  frontend:
    image: frontend
    ports:
      - 5000:5000
    container_name: frontend
  backend:
    image: backend
    ports:
      - 8080:8080
    container_name: backend
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
      - POSTGRES_PASSWORD=postgres
      
volumes:
  database:
```

## Exercise 2.9
docker-compose.yml
```yml
version: '2.5'

services:
  nginx:
    image: nginx:1.19-alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - 80:80
  postgres:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: postgres
    volumes:
      - database:/var/lib/postgresql/data
  redis:
    image: redis
  frontend:
    image: frontend
    ports:
      - 5000:5000
    container_name: frontend
  backend:
    image: backend
    ports:
      - 8080:8080
    container_name: backend
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
      - POSTGRES_PASSWORD=postgres
      
volumes:
  database:
```

## Exercise 2.10
docker-compose.yml
```yml
version: '2.5'

services:
  nginx:
    image: nginx:1.19-alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - 80:80
  postgres:
    image: postgres:13.2-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: postgres
    volumes:
      - database:/var/lib/postgresql/data
  redis:
    image: redis
  frontend:
    image: frontend
    ports:
      - 5000:5000
    container_name: frontend
  backend:
    image: backend
    ports:
      - 8080:8080
    container_name: backend
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
      - POSTGRES_PASSWORD=postgres
      
volumes:
  database:
```
Modify Dockerfile and build new image for Backend
```Dockerfile
FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN go build
RUN go test ./...
ENV REQUEST_ORIGIN=http://localhost
CMD ["./server"]
```
No changes were made to Frontend's Dockerfile
```Dockerfile
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
Modify nginx.conf to include other locations
```bash
events { worker_connections 1024; }

http {
    server {      
        listen 80;
        
        location / {
            proxy_pass http://frontend:5000/;
        }

        location /api/ {
            proxy_pass http://backend:8080/;
        }

        location /messages {
            proxy_pass http://backend:8080/messages;
        }

        location /ping {
            proxy_pass http://backend:8080/ping;
        }

        location /slow {
            proxy_pass http://backend:8080/slow;
        }
    }
}
```

## Exercise 2.11
Skipped, no project available to implement.

