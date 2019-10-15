Nama	: Arvian Eka Saputra

NIM		: 175410041

Kelas	: TI-9
________________________________________
## Pertemuan 5

[Deploying Your First Docker Container](https://katacoda.com/courses/docker/deploying-first-container)

1. Step 1-Running A Container

The first task is to identify the name of the Docker Image which is configured to run Redis. With Docker, all containers are started based on a Docker Image. These images contain everything required to launch the process; the host doesn't require any configuration or dependencies.

Jane can find existing images at registry.hub.docker.com/ or by using the command docker search <name>. For example, to find an image for Redis, you would use

    ```
    $ docker search redis
    NAME                             DESCRIPTION                                     STARS               OFFICIAL    AUTOMATED
    redis                            Redis is an open source key-value store that…   7412                [OK]
    bitnami/redis                    Bitnami Redis Docker Image                      129                 [OK]
    sameersbn/redis                                                                  77                  [OK]
    grokzen/redis-cluster            Redis cluster 3.0, 3.2, 4.0 & 5.0               61
    rediscommander/redis-commander   Alpine image for redis-commander - Redis man…   31                  [OK]
    kubeguide/redis-master           redis-master with "Hello World!"                30
    redislabs/redis                  Clustered in-memory database engine compatib…   23
    oliver006/redis_exporter          Prometheus Exporter for Redis Metrics. Supp…   18
    arm32v7/redis                    Redis is an open source key-value store that…   17
    redislabs/redisearch             Redis With the RedisSearch module pre-loaded…   17
    webhippie/redis                  Docker images for Redis                         10                  [OK]
    s7anley/redis-sentinel-docker    Redis Sentinel                                  9                   [OK]
    bitnami/redis-sentinel           Bitnami Docker Image for Redis Sentinel         8                   [OK]
    insready/redis-stat              Docker image for the real-time Redis monitor…   8                   [OK]
    redislabs/redisgraph             A graph database module for Redis               8                   [OK]
    arm64v8/redis                    Redis is an open source key-value store that…   6
    centos/redis-32-centos7          Redis in-memory data structure store, used a…   4
    redislabs/redismod               An automated build of redismod - latest Redi…   4                   [OK]
    circleci/redis                   CircleCI images for Redis                       2                   [OK]
    frodenas/redis                   A Docker Image for Redis                        2                   [OK]
    tiredofit/redis                  Redis Server w/ Zabbix monitoring and S6 Ove…   1                   [OK]
    runnable/redis-stunnel           stunnel to redis provided by linking contain…   1                   [OK]
    wodby/redis                      Redis container image with orchestration        1                   [OK]
    xetamus/redis-resource           forked redis-resource                           0                   [OK]
    cflondonservices/redis           Docker image for running redis                  0
    $ docker run -d redis:latest
    29d1ba77c4d82f28f5236d7804d19a74eede448ca734019a8218923026b7ffb4
    $ docker run -d redis:latest
    0adc431a8b52a2a2cd759a0e7f3130e9559140a18168681c1a9c0bbfc957567e
    $
    ```

2. Step 2-Finding Running Containers

Container yang diluncurkan berjalan di latar belakang, perintah docker ps mencantumkan semua kontainer yang sedang berjalan, gambar yang digunakan untuk memulai wadah dan waktu aktif.

    ```
    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS         NAMES
    0adc431a8b52        redis:latest        "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        6379/tcp         thirsty_carson
    29d1ba77c4d8        redis:latest        "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes        6379/tcp         vigilant_heisenberg
    $
    ```

3. Step 3-Accessing Redis

By default, Redis runs on port 6379 and other applications and library expect a Redis instance to be listening on the port.

### Protip

By default, the port on the host is mapped to 0.0.0.0, which means all IP addresses. You can specify a particular IP address when you define the port mapping, for example, -p 127.0.0.1:6379:6379

    ```
    $ docker run -d --name redisHostPort -p 6379:6379 redis:latest
    1cda87ab19135f65a0f7723524eeb8dbe3bf9e3bcfb248e61bf54947a891d56e
    $
    ```

4. Step 4-Accessing Redis

The problem with running processes on a fixed port is that you can only run one instance. We would prefer to run multiple Redis instances and configure the application depending on which port Redis is running on.

After experimenting, Jane discovers that just using the option -p 6379 enables her to expose Redis but on a randomly available port. She decides to test her theory using

    ```
    $ docker run -d --name redisDynamic -p 6379 redis:latest
    b161e67c697296fb725eec388ede91c52cb122a3515edd8ebfbcea7d2cb595f9
    $
    ```

While this works, she now doesn't know which port has been assigned. Thankfully, this is discovered via

    ```
    $ docker port redisDynamic 6379
    0.0.0.0:32768
    $
    ```

Jane also finds that listing the containers displays the port mapping information,

    ```
    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
    b161e67c6972        redis:latest        "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:32768->6379/tcp   redisDynamic
    1cda87ab1913        redis:latest        "docker-entrypoint.s…"   5 minutes ago        Up 5 minutes        0.0.0.0:6379->6379/tcp    redisHostPort
    0adc431a8b52        redis:latest        "docker-entrypoint.s…"   19 minutes ago       Up 19 minutes       6379/tcp                thirsty_carson
    29d1ba77c4d8        redis:latest        "docker-entrypoint.s…"   19 minutes ago       Up 19 minutes       6379/tcp                vigilant_heisenberg
    $
    ```

5. Step 5-Persisting Data

After working with containers for a few days, Jane realises that the data stored keeps being removed when she deletes and re-creates a container. Jane needs the data to be persisted and reused when she recreates a container.

Containers are designed to be stateless. Binding directories (also known as volumes) is done using the option -v <host-dir>:<container-dir>. When a directory is mounted, the files which exist in that directory on the host can be accessed by the container and any data changed/written to the directory inside the container will be stored on the host. This allows you to upgrade or change containers without losing your data.

Task
Using the Docker Hub documentation for Redis, Jane has investigated that the official Redis image stores logs and data into a /data directory.

Any data which needs to be saved on the Docker Host, and not inside containers, should be stored in /opt/docker/data/redis.

The complete command to solve the task is

    ```
    $ docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
    2913b97a5bb205239147666452eeda65eff3eed5bb64b788c5c402134e534142
    $
    ```

### Protip

Docker allows you to use $PWD as a placeholder for the current directory.

6. Step 6-Running A Container In The Foreground

Jane has been working with Redis as a background process. Jane wonders how containers work with foreground processes, such as ps or bash.

Previously, Jane used the -d to execute the container in a detached, background, state. Without specifying this, the container would run in the foreground. If Jane wanted to interact with the container (for example, to access a bash shell) she could include the options -it.

As well as defining whether the container runs in the background or foreground, certain images allow you to override the command used to launch the image. Being able to replace the default command makes it possible to have a single image that can be re-purposed in multiple ways. For example, the Ubuntu image can either run OS commands or run an interactive bash prompt using /bin/bash

Example
The command launches an Ubuntu container and executes the command ps to view all the processes running in a container.

    ```
    $ docker run ubuntu ps
    PID TTY          TIME CMD
        1 ?        00:00:00 ps
    ```

Using it allows Jane to get access to a bash shell inside of a container.

    ```
    $ docker run -it ubuntu bash
    root@51193e67a8d1:/#
    ```


[Deploy Static HTML Website as Container](https://katacoda.com/courses/docker/create-nginx-static-web-server)

1. Step 1-Create Dockerfile

Docker Images start from a base image. The base image should include the platform dependencies required by your application, for example, having the JVM or CLR installed.

This base image is defined as an instruction in the Dockerfile. Docker Images are built based on the contents of a Dockerfile. The Dockerfile is a list of instructions describing how to deploy your application.

    ```
    FROM nginx:alpine
    COPY . /usr/share/nginx/html
    ```

2. Step 2-Build Docker Image

The Dockerfile is used by the Docker CLI build command. The build command executes each instruction within the Dockerfile. The result is a built Docker Image that can be launched and run your configured app.

    ```
    $ docker build -t webserver-image:v1 .
    Sending build context to Docker daemon  3.072kB
    Step 1/2 : FROM nginx:alpine
    ---> 4d3c246dfef2
    Step 2/2 : COPY . /usr/share/nginx/html
    ---> 50116c5b4a64
    Successfully built 50116c5b4a64
    Successfully tagged webserver-image:v1
    $

    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    webserver-image     v1                  50116c5b4a64        34 seconds ago      21.2MB
    nginx               alpine              4d3c246dfef2        2 weeks ago         21.2MB
    redis               latest              4760dc956b2d        19 months ago       107MB
    ubuntu              latest              f975c5035748        19 months ago       112MB
    alpine              latest              3fd9065eaf02        21 months ago       4.14MB
    $
    ```

3. Step 3-Run

The built Image can be launched in a consistent way to other Docker Images. When a container launches, it's sandboxed from other processes and networks on the host. When starting a container you need to give it permission and access to what it requires.

For example, to open and bind to a network port on the host you need to provide the parameter -p <host-port>:<container-port>.

    ```
    $ docker run -d -p 80:80 webserver-image:v1
    bf8b8bb9146d862867e2b0ef3c61e0ae8c9475ab82c19499b6d250a337b30991
    $ curl docker
    <h1>Hello World</h1>
    $
    ```

[Building Container Images](https://katacoda.com/courses/docker/2)

1. Step 1-Base Images

All Docker images start from a base image. A base image is the same images from the Docker Registry which are used to start containers. Along with the image name, we can also include the image tag to indicate which particular version we want, by default, this is latest.

These base images are used as the foundation for your additional changes to run your application.

    ```
    FROM nginx:1.11-alpine
    ```

2. Step 2-Running Commands

With the base image defined, we need to run various commands to configure our image. There are many commands to help with this, the main commands two are COPY and RUN.

RUN <command> allows you to execute any command as you would at a command prompt, for example installing different application packages or running a build command. The results of the RUN are persisted to the image so it's important not to leave any unnecessary or temporary files on the disk as these will be included in the image.

COPY <src> <dest> allows you to copy files from the directory containing the Dockerfile to the container's image. This is extremely useful for source code and assets that you want to be deployed inside your container.

    ```
    COPY index.html /usr/share/nginx/html/index.html
    ```

3. Step 3-Exposing Ports

With our files copied into our image and any dependencies downloaded, you need to define which port application needs to be accessible on.

Using the EXPOSE <port> command you tell Docker which ports should be open and can be bound to. You can define multiple ports on the single command, for example, EXPOSE 80 433 or EXPOSE 7000-8000

    ```
    EXPOSE 80
    ```

4. Step 4-Default Commands

With the Docker image configured and having defined which ports we want accessible, we now need to define the command that launches the application.

The CMD line in a Dockerfile defines the default command to run when a container is launched.

    ```
    CMD ["nginx", "-g", "daemon off;"]
    ```

5. Step 5-Building Containers

After writing your Dockerfile you need to use docker build to turn it into an image. The build command takes in a directory containing the Dockerfile, executes the steps and stores the image in your local Docker Engine. If one fails because of an error then the build stops.

    ```
    $ docker build -t my-nginx-image:latest .
    Sending build context to Docker daemon  3.072kB
    Step 1/4 : FROM nginx:1.11-alpine
    ---> bedece1f06cc
    Step 2/4 : COPY index.html /usr/share/nginx/html/index.html
    ---> 9ad2dfdfc60d
    Step 3/4 : EXPOSE 80
    ---> Running in fb785661be2b
    Removing intermediate container fb785661be2b
    ---> fb37486eca34
    Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
    ---> Running in 19ca8e72468f
    Removing intermediate container 19ca8e72468f
    ---> a77dcc57fec0
    Successfully built a77dcc57fec0
    Successfully tagged my-nginx-image:latest
    $
    ```

6. Step 6-Launching New Image

With the image successfully created, you can now launch the container in the same way we described in the first scenario.

    ```
    $ docker run -d -p 80:80 my-nginx-image:latest
    ebb9fa5d2565d85636762195aa893ce635948ed8b3de661ca8717821a20ceb63
    $
    ```
