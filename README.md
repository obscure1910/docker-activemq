docker active-mq
================

This repository is a continuation of the good work on [rmohr/activemq](https://github.com/rmohr/docker-activemq)

### Build with BuildKit
    DOCKER_BUILDKIT=1 AMQ_VERSION=5.16.3 docker compose build

### Run container:
    docker run -d -p 61616:61616 -p 8161:8161 obscure1910/activemq:5.16.3

