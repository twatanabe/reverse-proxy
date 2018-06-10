# Traefik Reverse Proxy Lab

Utilize Traefik secure routing to navigate sub-domain web access to web app containers. Essencially structures network layer that connects abc.domain.com and xyz.domain.com accesses to associated web app pages.


<p align="center">
  <img src="https://raw.githubusercontent.com/code-badger/reverse-proxy/master/diagram.png">
</p>


### Requirements
- [Digital Ocean](https://www.digitalocean.com/products/droplets/) - Linux Server Droplet
- [Docker Compose](https://docs.docker.com/compose/overview/)
- [Docker Machine](https://docs.docker.com/machine/overview/)
- [Traefik](https://traefik.io/)
- Web App Container for access test, here using Go-Hello and Python-Hello

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
