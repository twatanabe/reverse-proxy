# Traefik Reverse Proxy Lab

Traefik networking lab for routing reverse proxy access to docker-machine deployed projects on global DNS Droplet server. Essentially structures network layer that connects xxx.site.com and yyy.site.com sub-domains to associated container apps.

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

