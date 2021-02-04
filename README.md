# Docker Valet for Linux

## Introduction

Docker Valet for Linux is a development environment for docker minimalists. No `/etc/hosts` file to manage, no ports to manage, no proxy configuration files. You can even share your sites publicly using local tunnels. Yeah, we like it too.

Docker Valet for Linux creates a _valet_ docker network on your system to always run [DnsMasq](https://en.wikipedia.org/wiki/Dnsmasq) and [traefik](http://traefik.io) in the background when your machine starts. With [DnsMasq](https://en.wikipedia.org/wiki/Dnsmasq) and [traefik](http://traefik.io) Docker Valet for Linux proxies all requests on the `*.dev` domain to point to docker web apps on your local machine.

## Installation

```shell
git clone https://github.com/besrabasant/docker-valet
cd docker-valet
script/bootstrap <interface> up # <interface> could be eth0 or your wifi interface
```

## Usage

Setup your docker apps to use the valet network and add labels to the service you want proxy. Check out the example docker-compose file below.

```yaml
version: 2

networks:
  valet:
    external: true

services:
  db:
    image: mysql
    
  web:
    image: marcusmyers/laravel
    networks:
      - valet
      - default
    labels:
      - "traefik.frontend.rule=Host:laravel.test"
      - "traefik.port=80"
      - "traefik.docker.network=valet"
    command: php artisan serve --host=0.0.0.0
```

## License

Docker Valet for Linux is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
