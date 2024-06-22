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

* Restarting Stopped Container

---
