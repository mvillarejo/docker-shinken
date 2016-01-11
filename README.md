# Docker Shinken

## Introduction
> Docker-shinken contains all the files required to build all the images required for Shinken to run in a docker environment separating every daemon in a image for easy escalation

## Folder Structure
* `custom_configs:` Pure Shinken configuration
    *   `certs:`
    *   `daemons:` shinken daemons configuration
    *   `modules:` and `packs`: common shinken configuration
    *   `config:` hosts, services, hostgroups, contacts, etc.


* `Shinken images:`
    * `shinken-base:` Shinken basic image used as template for all the rest of images
    * `shinken-arbiter:`
    * `shinken-broker`
    * `shinken-poller:`
    * `shinken-reactionner:`
    * `shinken-receiver:`
    * `shinken-scheduler:`
    * `shinken-thruk:`

* `shinken-config-checker:` Small container to validate shinken configuration


* `File docker-compose.yml:` Docker compose orchestrator file




## Check your config
     docker run -v $(PWD)/custom_configs:/sites/shinken shinken-config-checker


## Run
    docker-compose --x-networking up

Naviagate to:

* **Thruk:** http://[docker-machine-ip]:8000/
* **Shinken:** http://[docker-machine-ip]:8001/


     
## TODO
1. Redundancy/escalability and orchestration.
2. Ease configuration using thruk web interface
