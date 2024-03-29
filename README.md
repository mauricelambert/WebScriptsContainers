![WebScripts Logo](https://mauricelambert.github.io/info/python/code/WebScripts/small_logo.png "WebScripts logo")

# WebScripts containers - Docker

## WebScripts

### Description

WebScripts is a tool to run scripts and display the result in a Web Interface.

### Documenation

The documentation is available on [readthedocs](https://webscripts.readthedocs.io/en/latest/) and on my [github wiki](https://github.com/mauricelambert/WebScripts/wiki).

### Sources

Sources are available on my [github](https://github.com/mauricelambert/WebScripts).

## Containers

Three tags are available on [Docker hub](https://hub.docker.com/r/mauricelambert/webscripts).

1. The WebScripts Server (tag is the *version*, example: `3.0.11`)
2. Deployment of WebScripts Server with Apache and mod_wsgi (tag are *apache_\<version>*, example: `apache_3.0.11` and `lastest`, *default container*)
3. Deployment of WebScripts Server with Nginx as HTTPS proxy (tag is *nginx_\<version>*, example: `nginx_3.0.11`)

Dockerfiles are available on my [github](https://github.com/mauricelambert/WebScriptsContainers).

### WebScripts with Apache deployment using mod_wsgi

#### Docker hub

##### Demo

[![Deploy WebScripts - Youtube](https://img.youtube.com/vi/NhRpaRCNVVs/0.jpg)](http://www.youtube.com/watch?v=NhRpaRCNVVs)

*Deploy WebScripts with Docker - Youtube video*

##### Download

Using default docker image:

```bash
docker pull mauricelambert/webscripts
```

Using default docker image:

```bash
docker pull mauricelambert/webscripts:latest
```

Using specific version of WebScripts:

```bash
docker pull mauricelambert/webscripts:apache_3.0.11
```

##### Run

For test:

```bash
docker run --name WebScripts -d mauricelambert/webscripts:apache_3.0.11
```

For deployment:

```bash
docker run --name WebScripts --restart always -p 80:80/tcp -p 443:443/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/lib/data -d mauricelambert/webscripts:apache_3.0.11
```

#### Github

##### Clone

```bash
git clone https://github.com/mauricelambert/WebScriptsContainers.git
```

##### Build

NOTE: the volume is not compatible with the Nginx deployment and the basic WebScripts container because the owner is not the same, you should change the owner of each files to use the apache deployment volume with nginx container or the basic WebScripts container.

```bash
cd apache
docker build --build-arg PASS=<AdminPassword> -t apache_webscripts .
docker volume create apache_webscripts_data
```

##### Run

For test:

```bash
docker run --name WebScripts -d apache_webscripts
```

For deployment:

```bash
docker run --name WebScripts --restart always -p 80:80/tcp -p 443:443/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/data -d apache_webscripts
```

### WebScripts with Nginx as HTTPS proxy

#### Docker hub

##### Download

```bash
docker pull mauricelambert/webscripts:nginx_3.0.11
```

##### Run

For test:

```bash
docker run --name WebScripts -it --rm mauricelambert/webscripts:nginx_3.0.11
```

For deployment:

```bash
docker run --name WebScripts --restart always -p 80:80/tcp -p 443:443/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/data -d mauricelambert/webscripts:nginx_2.4.9
```

#### Github

##### Clone

```bash
git clone https://github.com/mauricelambert/WebScriptsContainers.git
```

##### Build

NOTE: the volume is compatible with the basic WebScripts container but not with the Apache deployment because the owner is not the same, you should change the owner of each files to use the nginx deployment volume with apache container.

```bash
cd nginx
docker build --build-arg PASS=<AdminPassword> -t nginx_webscripts .
docker volume create webscripts_data
```

##### Run

For test:

```bash
docker run --name WebScripts -it --rm nginx_webscripts
```

For deployment:

```bash
docker run --name WebScripts --restart always -p 80:80/tcp -p 443:443/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/data -d nginx_webscripts
```

### Simple WebScripts

**The connection is not secure** (the socket is not secure by SSL. It's HTTP not HTTPS).

#### Docker hub

##### Download

```bash
docker pull mauricelambert/webscripts:3.0.11
```

##### Run

For test:

```bash
docker run --name WebScripts -it --rm mauricelambert/webscripts:3.0.11
```

For deployment:

```bash
docker run --name WebScripts --restart always -p 80:8000/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/data -d mauricelambert/webscripts:3.0.11
```

#### Github

##### Clone

```bash
git clone https://github.com/mauricelambert/WebScriptsContainers.git
```

##### Build

NOTE: the volume is compatible with the Nginx deployment but not with the Apache deployment because the owner is not the same, you should change the owner of each files to use the basic WebScripts deployment volume with apache container.

```bash
cd simple
docker build --build-arg PASS=<AdminPassword> -t webscripts .
docker volume create webscripts_data
```

##### Run

For test:

```bash
docker run --name WebScripts -it --rm webscripts
```

For deployment:

```bash
docker run --name WebScripts --restart always -p 80:8000/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/data -d webscripts
```

### Useful commands

#### Console output

```bash
docker logs <dockerID>
```

#### Pull files

```bash
docker cp <dockerID>:/usr/src/WebScripts/data data
docker cp <dockerID>:/usr/src/WebScripts/hardening/audit.html audit.html
docker cp <dockerID>:/usr/src/WebScripts/logs logs
```

#### Interactive Bash in the container

```bash
docker exec -it WebScripts bash
```
