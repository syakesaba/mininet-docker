# Mininet Docker Image
This contains the files to construct a mininet docker image that can execute
mininet simulated networks.

## Privileged Mode
It is important to run this container in Privileged mode (`--privileged`) so that if can manipulate the network interface properties and devices. I suspect this can also be achieved with the capabilities (`--cap-add`) features of docker, but this has not been investigated.

## Overview
This container by default executes the the mininet (`mn`) executable with the options to the docker run command passed as parameters to the mininet processes.

## Docker Compose
A sample docker-compose entry for this container
```
fabric:
    image: syakesaba/mininet
    privileged: true
    stdin_open: true
    tty: true
    command: --topo=single,3 --mac
```

or

```
ryu:
    image: osrg/ryu
    command: ryu-manager ryu.app.simple_switch
    expose:
        - "6633/udp"
        - "6633/tcp"

fabric:
    image: syakesaba/mininet
    privileged: true
    stdin_open: true
    tty: true
    command: --topo=single,3 --mac
    links:
        - ryu:ryu
    environment:
        REMOTE_CONTROLLER: ryu
```

and run "docker-compose run --rm fabric"
