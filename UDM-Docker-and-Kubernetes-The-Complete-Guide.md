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
docker start -a xxxx

docker run = docker create + docker start
```

* Removing Stopped Container
```
docker system prune
```

* Retrieving Logs Outputs
```
docker logs xxxx
```

* Stopping Containers
```
docker stop xxx
```

![image](https://github.com/loonloon/Notes/assets/5309726/30b21822-4696-4eac-88f3-f1dc4b0c7b81)

```
docker kill xxx
```

![image](https://github.com/loonloon/Notes/assets/5309726/dd1a3339-891e-4494-896d-1ba8c185e874)

* Executing Commands in Running Containers

![image](https://github.com/loonloon/Notes/assets/5309726/54836f61-84f8-4b3a-a61b-e4d4a80835c3)

---
