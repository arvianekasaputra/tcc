Nama	: Arvian Eka Saputra

NIM		: 175410041

Kelas	: TI-9
________________________________________
## Pertemuan 5

[Link 1](https://katacoda.com/courses/docker/deploying-first-container)

1. Step 1

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

2. Step 2
    ```
    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS         NAMES
    0adc431a8b52        redis:latest        "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        6379/tcp         thirsty_carson
    29d1ba77c4d8        redis:latest        "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes        6379/tcp         vigilant_heisenberg
    $
    ```