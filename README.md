# Prowlarr VPN

This is a fork of Hotio's Prowlarr Docker image, with added Wireguard (and Privoxy) support.

Everything's updated automatically.

All credits goes to Hotio :)!

## Image Overview

There are three Docker images to choose from.

The tags are "[release](https://github.com/vp-en/prowlarr-vpn/tree/release)", "[testing](https://github.com/vp-en/prowlarr-vpn/tree/testing)", "[nightly](https://github.com/vp-en/prowlarr-vpn/tree/nightly)", and "[PR](https://github.com/vp-en/prowlarr-vpn/tree/pr)" (pull requests).

The "release" tag is for official "stable" releases, the "nightly" tag is for "unstable" releases (rebuilt with every commit to the Prowlarr develop branch), and the PR tag is built with every commit to the most recent pull request branch on the official Prowlarr repository.


## Using the images

**Release**

`docker pull ghcr.io/vp-en/prowlarr-vpn:release`


**Nightly**

`docker pull ghcr.io/vp-en/prowlarr-vpn:nightly`


For the **PR** images, you would have to look through [here](https://github.com/vp-en/prowlarr-vpn/pkgs/container/prowlarr-vpn/versions), and find the images labeled `pr-`.


## Enabling Wireguard

To enable Wireguard, follow the same steps as in [Hotio's qBittorrent images](https://hotio.dev/containers/qbittorrent/#wireguard-vpn-support).


## Docker Compose example


``` yaml
version: "3.9"

services:
  prowlarr-vpn:
    container_name: prowlarr-vpn
    image: ghcr.io/vp-en/prowlarr-vpn:release
    ports:
      - "9696:9696"
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Etc/UTC
      - WEBUI_PORTS=9696/tcp,9696/udp
      - VPN_ENABLED=true
      - VPN_CONF=wg0
      - VPN_IP_CHECK_DELAY=5
      - PRIVOXY_ENABLED=false
    volumes:
      - /your/path/here:/config
    cap_add:
      - NET_ADMIN
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv6.conf.all.disable_ipv6=0
    restart: unless-stopped
```

## Testing your connection

When the `prowlarr-vpn` container is up and running, run the following command to see if your VPN connection is working:

```
docker run --rm --network=container:prowlarr-vpn alpine:3.14 sh -c "apk add wget && wget -qO- https://ipinfo.io"
```

This will download a lightweight Alpine v3.14 image, route its network through the `prowlarr-vpn` container, and output the current IP address.

If it has the same IP as your VPS/server, you may have done something incorrectly. If the IP address corresponds to the VPN you specified in your `wg.conf`, everything is as it should be.

## Troubleshooting

If you do not want to use IPv6, want to disable IPv6, or if the docker reports IPv6 errors, change 'net.ipv6.conf.all.disable ipv6=0' to 'net.ipv6.conf.all.disable ipv6=1'.

## Support

You can try to get support on the [Hotio discord](https://hotio.dev/discord), but you won't likely get support for this Docker image. 

However, if you have problems configuring Wireguard, they may be able to assist you if you ask nicely:)

## Donate

You can show your support by giving Hotio a star on Hotio's [Docker Hub](https://hub.docker.com/u/hotio) or/and [GitHub](https://github.com/hotio), it's also possible to make a [donation](https://hotio.dev/donate) to Hotio!
