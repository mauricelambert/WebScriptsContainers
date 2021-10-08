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

1. The WebScripts Server
2. Deployment of WebScripts Server with Apache and mod_wsgi
3. Deployment of WebScripts Server with Nginx as HTTPS proxy

Dockerfiles are available on my [github](https://github.com/mauricelambert/WebScriptsContainers).

### WebScripts with Apache deployment using mod_wsgi

#### Docker hub

##### Download

```bash
docker pull mauricelambert/webscripts:apache_2.1.4
```

##### Launch

```bash
docker run -d mauricelambert/webscripts:apache_2.1.4
```

#### Github

##### Clone

```bash
git clone https://github.com/mauricelambert/WebScriptsContainers.git
```

##### Build

NOTE: the volume is not compatible with the Nginx deployment and the basic WebScripts container because the owner is not the same.

```bash
cd apache
docker build --build-arg PASS=<AdminPassword> -t apache_webscripts .
docker volume create apache_webscripts_data
```

##### Run

```bash
docker run -p 80:80/tcp -p 443:443/tcp --mount type=volume,source=apache_webscripts_data,target=/usr/src/WebScripts/lib/python3.9/site-packages/WebScripts/data -d apache_webscripts
```

### WebScripts with Nginx as HTTPS proxy

#### Docker hub

##### Download

```bash
docker pull mauricelambert/webscripts:nginx_2.1.4
```

##### Launch

```bash
docker run -it --rm mauricelambert/webscripts:nginx_2.1.4
```

```bash
docker run -d mauricelambert/webscripts:nginx_2.1.4
```

#### Github

##### Clone

```bash
git clone https://github.com/mauricelambert/WebScriptsContainers.git
```

##### Build

NOTE: the volume is compatible with the basic WebScripts container but not with the Apache deployment because the owner is not the same.

```bash
cd nginx
docker build --build-arg PASS=<AdminPassword> -t nginx_webscripts .
docker volume create webscripts_data
```

##### Run

```bash
docker run -p 80:80/tcp -p 443:443/tcp --mount type=volume,source=webscripts_data,target=/usr/src/WebScripts/lib/python3.9/site-packages/WebScripts/data -d nginx_webscripts
```

### Simple WebScripts

**The connection is not secure** (the socket is not secure by SSL. It's HTTP not HTTPS).

#### Docker hub

##### Download

```bash
docker pull mauricelambert/webscripts:2.1.4
```

##### Launch

```bash
docker run -it --rm mauricelambert/webscripts:2.1.4
```

```bash
docker run -d mauricelambert/webscripts:2.1.4
```

#### Github

##### Clone

```bash
git clone https://github.com/mauricelambert/WebScriptsContainers.git
```

##### Build

NOTE: the volume is compatible with the Nginx deployment but not with the Apache deployment because the owner is not the same.

```bash
cd simple
docker build --build-arg PASS=<AdminPassword> -t webscripts .
docker volume create webscripts_data
```

##### Run

```bash
docker run -p 80:8000/tcp --mount type=volume,source=webscripts_data,target=/usr/src/WebScripts/lib/python3.9/site-packages/WebScripts/data -d webscripts
```

### Useful commands

#### Console output

```bash
docker logs <dockerID>
```

#### Pull files

```bash
docker cp <dockerID>:/usr/src/WebScripts/lib/python3.9/site-packages/WebScripts/data data
docker cp <dockerID>:/usr/src/WebScripts/audit.html audit.html
docker cp <dockerID>:/usr/src/WebScripts/logs logs
```

#### Interactive Bash in the container

```bash
docker exec -it <dockerID> bash
```
