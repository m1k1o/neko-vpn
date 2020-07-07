# neko-vpn

Use VPN in [n.eko](https://github.com/nurdism/neko), modified by [m1k1o](https://github.com/m1k1o/neko), including [Traefik](https://docs.traefik.io/)-Ready docker-compose. 

## Install

You will need to have [Docker](https://docs.docker.com/engine/install/ubuntu/) and [Docker-compose](https://docs.docker.com/compose/install/) installed.

1. Clone this repository with submodules:

```sh
git clone --recurse-submodules https://github.com/m1k1o/neko-vpn
cd neko-vpn
```

2. Copy `.env.example` to `.env` and modify:

```sh
cp .env.example .env
```

3. Copy all `.ovpn` files into `vpn/` folder.

If you are using authentication in your VPN files (i hope you do so), you might want to include username and password as well. Please read tutorial [here](https://github.com/m1k1o/ovpn-nodejs/tree/f62621f5440b717b3e3ae344ab2476f5571e1f87#openvpn-authentication) how to do it.

### Publish port

If you just want to publish port for neko or use custom reverse proxy, this is your variant.

```sh
docker-compose up -d
```

**TIP:** If you are using reverse proxy, you might want to have published your port only for loopback. Use `HTTP_PORT="127.0.0.1:80"`. Now you can connect to container only from your maschine.

### Use with traefik

If you are already using Traefik, you might want to add this container to your existing domains. Please refer to `.env` file for further information. Maybe it will fit your use case.

```sh
docker-compose up -f docker-compose.traefik.yml -d
```

## Connect to VPN
You can connect to this VPN service using Proxy `vpn:3128`.

If you want your container to use VPN's routes, add [this](https://docs.docker.com/compose/compose-file/#network_mode) to docker-compose:

```yml
  my-service:
    image: "my-image"
    network_mode: "service:vpn"
```

Remember, since your container will be using network of VPN, traffic to published ports will be routed through VPN as well. Only local traffic will not, so you'll need reverse proxy, e.g. Traefik. It can handle now also TCP and UDP connections.
