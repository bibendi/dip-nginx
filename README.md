# Dip Nginx

[Nginx proxy](https://github.com/nginx-proxy/nginx-proxy) configuration for [Dip infra services](https://github.com/bibendi/dip).

## Usage

1. Add a service to the application's `dip.yml`.

```yaml
infra:
  nginx:
    git: https://github.com/bibendi/dip-nginx.git
```

2. Add a network to the application's `docker-compose.yml`.

```yaml
networks:
  nginx-net:
    name: ${DIP_INFRA_NETWORK_NGINX}
    external: true
```

Where `DIP_INFRA_NETWORK_NGINX` is a special variable that is set by Dip.

3. Add a `VIRTUAL_HOST` environment variable and the `nginx-net` network to a Docker Compose service.

```yaml
services:
  app:
    environment:
      VIRTUAL_HOST: some.lvh.me
    networks:
      - default
      - nginx-net
```

4. Start infra services.

```sh
dip infra up
```

5. Start the app

```sh
dip up
```

6. Send a request to the app

```shell
curl -L http://some.lvh.me
```

7. In case of a container to container communication use the following command:

```shell
# inside a container
curl -L http://host.docker.internal -H 'Host: some.lvh.me'
```
