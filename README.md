# Traefik Reverse Proxy Lab

Traefik networking lab for routing reverse proxy access to docker-machine deployed projects on global DNS Droplet server. Essentially structures network layer that connects xxx.site.com and yyy.site.com sub-domains to associated container apps.


![alt text](https://raw.githubusercontent.com/code-badger/reverse-proxy/master/diagram.png)


### Requirements
- Droplet Server
- docker-compose
- docker-machine

## How to use

Run Traefik
```
docker-machine use <machine-name>
cd traefik
docker-compose up
```

Run apps
```
cd go-hello
docker-compose up
```

## Ports

Traefik: 8080
GoHello: 3000
PythonHello: 5000

## Apply Traefik Routing

Traefik network will run on <traefik_service>_<traefik_network> network defined in the traefik docker-compose.yml. In this project it will run on the "traefik_reverseproxy" network.
Use following config for routing apps using Traefik network
```
version: '3'
services:
    gohello:
        build: .
        image: codebadger/gohello
        networks:
            - reverseproxy
            - default
        labels:
            - "traefik.enable=true"
            - "traefik.backend=gohello"
            - "traefik.frontend.rule=Host:gohello.wtakumi.io"
            - "traefik.docker.network=traefik_reverseproxy"
        ports:
        - "3000:3000"

networks:
    reverseproxy:
        external:
            name: traefik_reverseproxy
```
