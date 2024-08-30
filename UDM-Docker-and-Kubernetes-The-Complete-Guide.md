#### Section 2 Manipulating Containers with the Docker Client ####

* Docker Run in Detail
![image](https://github.com/loonloon/Notes/assets/5309726/ebe2d1c8-6e3b-4bae-a377-cf9e1134cbfb)

* Listing Running Containers
```
//Lists only the running containers
docker ps

//Lists all containers, including those that are stoppe
docker ps --all 
```

* Container Lifecycle
```
docker create hello-world

/*
`-a` it attaches the container's standard output (stdout) and standard error (stderr) to your terminal,
allowing you to view the container's output live
*/
docker start -a docker_id

docker run = docker create + docker start
```

* Removing Stopped Container
```
docker system prune
```

* Retrieving Logs Outputs
```
docker logs docker_id
```

* Stopping Containers
```
docker stop docker_id
```

![image](https://github.com/loonloon/Notes/assets/5309726/30b21822-4696-4eac-88f3-f1dc4b0c7b81)

```
docker kill docker_id
```

![image](https://github.com/loonloon/Notes/assets/5309726/dd1a3339-891e-4494-896d-1ba8c185e874)

* Executing Commands in Running Containers
```
docker exec -it docker_id redis-cli
```
![image](https://github.com/loonloon/Notes/assets/5309726/54836f61-84f8-4b3a-a61b-e4d4a80835c3)

![image](https://github.com/loonloon/Notes/assets/5309726/67bc838a-cae8-4a83-a825-797b71d5281c)

* Getting a Command Prompt in a Container
```
//works with an existing, running container
docker exec -it docker_id sh

//creates and starts a new container
docker run -it busybox sh
```

---

#### Section 3 Building Custom Images Through Docker Server ####

![image](https://github.com/loonloon/Notes/assets/5309726/67838788-daeb-48a2-bfde-6b20bddacd7c)

* Building a Dockerfile
```
# Use an existing docker image as a base
FROM alpine

# Download and install a dependency
RUN apk add --update redis

# Tell the image what to do when it starts
# as a container
CMD ["redis-server"]
```

![image](https://github.com/loonloon/Notes/assets/5309726/461ed68a-9d79-4a0b-a5b1-0e7eeff88cc1)

* Tagging an Image
```
docker build -t docker_id/project_name:latest .
```

* Manual Image Generation with Docker Commit
```
docker commit -c 'CMD ["redis-server"]' docker_id
```
---

#### Section 4 Making Real Projects with Docker ####

* Dockerfile
```
# Use an official Node.js runtime as a base image, with Alpine Linux for a minimal footprint
FROM node:14-alpine

# Set the working directory inside the container
WORKDIR /usr/app

# Copy only the package.json file to the working directory
# This allows Docker to cache the npm install step if dependencies haven't changed
COPY ./package.json ./

# Install project dependencies from package.json
RUN npm install

# Copy the rest of the application files to the working directory
# This step occurs after npm install to leverage Docker's layer caching
COPY ./ ./

# Specify the default command to run the application
CMD ["npm", "start"]
```

```
docker build -t loonloon/simpleweb .
```

* Container Port Mapping
```
docker run -p 5000:8080 loonloon/simpleweb
```
![image](https://github.com/user-attachments/assets/7c28309d-3e48-408e-8477-13e729abceb6)

![image](https://github.com/user-attachments/assets/e5406a4e-abf0-4c1f-b049-75529a72102f)

---

#### Section 5 Docker Compose with Multiple Local Containers ####

![image](https://github.com/user-attachments/assets/aca292b6-f072-40c3-980d-9e0d8597640c)

* Introducing Docker Compose

![image](https://github.com/user-attachments/assets/e5455cee-ecd2-4fe1-989d-01dcf9ad5943)

* Docker Compose File

![image](https://github.com/user-attachments/assets/ddcd302c-adfc-42eb-bd8f-a3339ae04166)

```
version: '3'

services:
  redis-server:
    image: 'redis'  # This service uses the official Redis image from Docker Hub

  node-app:
    build: .  # This service builds the Docker image for the Node.js app using the Dockerfile in the current directory
    ports:
      - "4001:8081"
      # Maps port 4001 on the host to port 8081 inside the container, allowing access to the Node.js app from outside the container
```

* Networking with Docker Compose

![image](https://github.com/user-attachments/assets/cef7594a-babe-48b3-b64e-fd6e620e5eee)

In Docker Compose, all containers defined within the same docker-compose.yml file are automatically placed in the same network, which Docker creates by default for that Compose project. This network allows containers to communicate with each other freely using the service names as hostnames.

For example, in your Docker Compose setup:

The node-app service can access the redis-server by using redis-server as the hostname.
Similarly, the redis-server can access the node-app using node-app as the hostname.

* Docker Compose Commands

![image](https://github.com/user-attachments/assets/5cbb7cec-4269-4107-8d21-c9811c24eded)

---

