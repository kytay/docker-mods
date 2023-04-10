# Cloudflare Dynamic DNS Updater - Docker mod for SWAG
This mod provides additional capability for SWAG to update cloudflare DNS type A records via flarectl. 

For this mod to function correctly, make sure the cloudflare api token or api key is defined in `/config/dns-conf/cloudflare.ini`. 

To enable this mod, set an environment variable `DOCKER_MODS=linuxserver/swag-cloudflare-dyndns-updater`

## Example
```yaml
version: "3.8"
services:
  swag:
    image: lscr.io/linuxserver/swag:latest
    cap_add:
      - NET_ADMIN
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - URL=example.com
      - VALIDATION=dns
      - SUBDOMAINS=wildcard
      - DNSPLUGIN=cloudflare
      - EXTRA_DOMAINS=example.io,example.net
      - #...
      - DOCKER_MODS=linuxserver/swag-cloudflare-dyndns-updater # enable mod
      - CLOUDFLARE_HOSTED_DOMAINS=example.com,example.io,example.net # domain name to dyndns update
      - CLOUDFLARE_PROXY=true # optional - allow cloudflare proxied service
    volumes:
      - /path/to/appdata/config:/config
    ports:
      - 443:443
    restart: unless-stopped
```

## Configuration - DNS Config - cloudflare.ini

Define either `dns_cloudflare_email` with `dns_cloudflare_api_key`, or `dns_cloudflare_api_token` in `/config/dns-conf/cloudflare.ini`

## Configuration - Environment Variables

`CLOUDFLARE_HOSTED_DOMAINS` - list of comma separated domain names hosted on cloudflare dns servers.

```bash
# example
CLOUDFLARE_HOSTED_DOMAINS=example.com,example.io,example.net
```

`CLOUDFLARE_PROXY` - set `true` to enable cloudflare managed proxied service, to protect your endpoint ip addresses. Default is set to `false`. For more information, refer to [cloudflare's proxied-dns-records](https://developers.cloudflare.com/dns/manage-dns-records/reference/proxied-dns-records/)

```bash
# example
CLOUDFLARE_PROXY=true
```
