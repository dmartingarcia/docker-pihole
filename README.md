# docker-pihole

It is part of a [set of repositories](https://github.com/search?q=user%3Admartingarcia+docker) that contain dockerised environments for small applications.

In this case, it contains a self-efficient *Pihole* setup.

## How to use it

```
cp .env.example .env
docker-compose up -d
```

And browse into `http://localhost:9009` in order to see the `pihole` admin page.

## Traefik integration`

It also contains a Traefik integration, that interact with this reverse proxy, routing request to each container in case it matches the specified domain

There's multiple subdomains exposed:
  - pihole.your.domain.com

## .env setup

It contains a basic set of variables like:

- Credentials
- http host that uses Traefik to match requests

Please take a look on that and :warning: create your own credentials :warning: in case you want to expose it to the public.

## Docker Traefik

If you want to use this Traefik integration, [take a look at this repository](https://github.com/dmartingarcia/docker-traefik)

## Port conflicts

### My port 53 is already binded:

Check which service is using it: `sudo lsof -i :53`

- If it's systemd [check this article](https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu)


I'd write which command combination fixed it for me (Ubuntu 20.04).

```
sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf
sudo sh -c 'rm /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'
systemctl restart systemd-resolved
```