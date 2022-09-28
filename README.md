# docker-traefik

> Traefik development environment for routing VEuPathDB BRC containers

This project contains a `docker-compose.yml` file to test routing Docker containers through Traefik. This environment is very similar to production web VEuPathDB BRC containers.

### Quick Start
1. Clone or directly download the `docker-compose.yml`

2. Start Traefik's compose file: `docker-compose up -d`

3. Specify Traefik's network as external in **your** compose file on the top-level `networks` key
```
networks:
  traefik:
    external: true
```

4. Don't forget to reference Traefik's network on your web container's service-level `networks` key
```
    networks:
      - traefik
```

5. Add Traefik labels for your web container replacing `examplerouter` with a _unique_ router name. Note that the router name `api` is in use by Traefik itself. It is not a bad idea to be explicit on which port Traefik should use on your container even if port detection works.
```
    labels:
      traefik.http.routers.examplerouter.rule: Host(`www.example.org`)
      traefik.http.routers.examplerouter.entrypoints: websecure
      traefik.http.routers.examplerouter.tls: true
      traefik.http.services.examplerouter.loadbalancer.server.port: 80
      traefik.docker.network: traefik
```

### Notes
- By default, Traefik will expose the dashboard here: https://traefik.local.apidb.org:8443/dashboard/#/
- `*.local.apidb.org` should resolve to `127.0.0.1` otherwise add an entry in your hosts file to resolve locally

## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
