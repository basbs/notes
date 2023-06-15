# Docker cheat-sheet

- [Docker cheat-sheet](#docker-cheat-sheet)
  - [Docker Tips](#docker-tips)
    - [Filter and format the output of `docker images`](#filter-and-format-the-output-of-docker-images)
    - [View the CPU and memory usage of all the running container](#view-the-cpu-and-memory-usage-of-all-the-running-container)
    - [Access host machine's service from docker containers](#access-host-machines-service-from-docker-containers)
    - [Detach from a container without stopping it](#detach-from-a-container-without-stopping-it)
    - [Remove all stopped containers](#remove-all-stopped-containers)
    - [Restrict memory and CPU to container](#restrict-memory-and-cpu-to-container)
    - [Run commands in container](#run-commands-in-container)
    - [Start a container as root](#start-a-container-as-root)
    - [To use host network](#to-use-host-network)
  - [Docker secret Tips](#docker-secret-tips)
    - [Get the secrets into ENVs or to Spring boot](#get-the-secrets-into-envs-or-to-spring-boot)
  - [Docker Swarm](#docker-swarm)
    - [What is it?](#what-is-it)
    - [Features](#features)
    - [Re-deploy the service when compose file is modified.](#re-deploy-the-service-when-compose-file-is-modified)


## Docker Tips


### Filter and format the output of `docker images`
```
docker images --filter reference='imagename*' --format '{{.Repository}}:{{.Tag}}'
```
- More details on filter and format can be found [here](https://docs.docker.com/engine/reference/commandline/images/#filtering).
- Most 


### View the CPU and memory usage of all the running container
```
docker stats
```

### Access host machine's service from docker containers
- in docker command `docker run --add-host="localhost:192.168.2.XX"`
- in docker compose
    ```
    version: '2'
    services:
    web:
        build: .
        ports:
        - '80:80'
        extra_hosts:
        - "localhost:192.168.2.XX"
    ```

### Detach from a container without stopping it
```
TTY allocated (-t)
stdin left open (-i)
```
- ^P^Q(ctrl+p ctrl+q) does work, BUT only when -t and -i is used to launch the container.

- ctrl+c does work, BUT only when -t (without -i) is used to launch the container.

- Third option is to detach without killing the container though; you need another shell. 
In summary, running this in another shell detached and left the container running `pkill -9 -f 'docker.*attach'`
Why? Because you're killing the process that connected you to the container, not the container itself.

### Remove all stopped containers
```
docker rm $(docker ps --filter status=exited -q)
```

### Restrict memory and CPU to container
- in version 2.X
    ```
    version: "2.2"
    services:
    app:
        image: foo
        cpus: "0.5"
        mem_limit: 23m
    ```

### Run commands in container
```
version: "3"
services:
 server:
   image: alpine
   command: sh -c "echo "from container"" 
```

### Start a container as root
```
services:
    demo:
        build: .
        user: root
        ports:
            - "9090:9090"
        depends_on:
            - db
```

###  To use host network 
```
version: "3"
services:
  web:
    image: conatinera:latest
    network_mode: "host"        
    restart: on-failure
```

## Docker secret Tips

### Get the secrets into ENVs or to Spring boot
- To put docker-secret file contents into ENV is currently not possible. 
    - Tried pointing the ENV to file location. Thinking that docker would magically replace the path with contents __DID NOT WORK__
    - Tried the env_expand.sh [from](https://gist.github.com/bvis/b78c1e0841cfd2437f03e20c1ee059fe).
        ```
        COPY env_expand.sh /env_expand.sh
        RUN chmod +x /env_expand.sh
        ENTRYPOINT "run.sh"

        --- run.sh ----
        #!/bin/sh
        source /env_expand.sh
        java -jar mdc-rest.jar
        ```
 - Above approach never tried to replace the ENV values with actual secret value. So This also __DID NOT WORK__
 - Added `spring.config.import=optional:configtree:/run/secrets/` into application.properties. Then refered the secret in properties as `${name}`. __It worked__
 - Only caveat is when the secret names contain underscore(_) the value were not picked up. as soon as i replaced with the simple names. It seem to work.[More details](https://stackoverflow.com/questions/70007676/how-to-handle-docker-secrets-in-application-properties-files)

> Note that if you are using secrets, they are only available after the entry point. You would have to use CMD instead of ENTRYPOINT if you want to copy or link your secret at startup

## Docker Swarm

### What is it?
- docker version 1.12, released in late July 2016, integrates Docker Engine and Swarm along with some new orchestration features, to create a platform similar to other container platforms such as Kubernetes.
- Swarm Mode allows you to combine a set of Docker hosts into a swarm, providing a fault‑tolerant, self‑healing, decentralized architecture.
- The new platform also makes it easier to set up a Swarm cluster, secures all nodes with a key, and encrypts all communications between nodes with TLS.
- the Docker API has been expanded to be aware of services, which are sets of containers that use the same image (similar to services in Docker Compose, but with more features).

### Features
- Create and scale services.
- Do rolling updates.
- Create health checks.
- DNS Service discovery(built in)
- Load balancing (built in)
- Can also setup cluster-wide overlay networks.

### Re-deploy the service when compose file is modified.