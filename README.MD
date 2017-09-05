# Yandex.Disk docker image

[![Docker Automated buil](https://img.shields.io/docker/automated/deanar/yandex.disk.svg)](https://hub.docker.com/r/deanar/yandex.disk/)
[![Docker Build Statu](https://img.shields.io/docker/build/deanar/yandex.disk.svg)](https://hub.docker.com/r/deanar/yandex.disk/)
[![Docker Pulls](https://img.shields.io/docker/pulls/deanar/yandex.disk.svg)](https://hub.docker.com/r/deanar/yandex.disk/)


**This build does not need to input login and password in the startup parameters or environment variables!**


## How it works

1) Run container using interactive mode with mounted directory for configuration
2) Obtain OAuth token using your login and password
3) Stop container
4) Run container in daemon mode with mounted configuration directory which contains auth token  


### Run directly

Obtain OAuth token  
(leave default path to Yandex.Disk folder or change it in volumes option too)

    docker run --rm -i -t \
    --volume=$(pwd)/config:/root/.config/:rw \
    --volume=$(pwd)/data:/root/Yandex.Disk:rw \
     deanar/yandex.disk yandex-disk setup

Run docker container to sync

    docker run -d --name ya.disk \
    --volume=$(pwd)/config:/root/.config/:rw \
    --volume=$(pwd)/data:/root/Yandex.Disk:rw \
     deanar/yandex.disk


Stop it `docker stop ya.disk`  
and remove `docker rm ya.disk` if no longer needed

### Run via compose file

_docker-compose.yml_

    version: "2"
    
    services:
      ya.disk:
        image: deanar/yandex.disk
        volumes:
          - ./config:/root/.config
          - ./data:/root/Yandex.Disk

Obtain OAuth token  
(leave default path to Yandex.Disk folder or change it in volumes section too)

    docker-compose run --rm ya.disk yandex-disk setup
        
Then you can run service
    
    docker-compose up -d
    
Stop it `docker-compose stop`
    
and remove `docker-compose rm` if no longer needed
    
## Configuration

If you need custom options, please refer to official documentation to setup config file `config/yandex-disk/config.cfg`

https://yandex.ru/support/disk/cli-clients.html 


## NB

If you run it on MacOS via boot2docker, remember that volumes can be mounted only inside home dir.