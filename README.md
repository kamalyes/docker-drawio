## Introduction

[draw.io](https://github.com/kamalyes/docker-drawio) is an open source whiteboarding / diagramming software application. This project contains various docker implementations of draw.io and associated tools:

* draw.io docker image that is always up-to-date with draw.io releases

## Description

The Dockerfile builds from `tomcat:9-jre11` (see <https://hub.docker.com/_/tomcat/>)

**Note: Starting from version 16.5.3, alpine and debian images are no longer maintained. We changed to a single image that uses the tomcat image with the least security vulnerabilities.**

## Features

* Based on Tomcat so it can be used directly or behind a reverse-proxy
* Self-Signed certificate autogen
* Let's encrypt certificate autogen
* Support SSL Keystore mount to `/user/local/tomcat/.keystore`

## Quick Start

Run the container.

```bash
docker run -it --rm --name="draw" -p 8080:8080 -p 8443:8443 kamalyes/drawio
```

Start a web browser session to `http://localhost:8080` or `https://localhost:8443`

## Environment variables

* **LETS_ENCRYPT_ENABLED**: Enables Let's Encrypt certificate instead of self-signed; default `false`
* **PUBLIC_DNS**: DNS domain to be used as certificate "CN" record; default `draw.example.com`
* **ORGANISATION_UNIT**: Organisation unit to be used as certificate "OU" record; default `Cloud Native Application`
* **ORGANISATION**: Organisation name to be used as certificate "O" record; default `example inc`
* **CITY**: City name to be used as certificate "L" record; default `Paris`
* **STATE**: State name to be used as certificate "ST" record; default `Paris`
* **COUNTRY_CODE**: Country code to be used as certificate "C" record; default `FR`
* **KEYSTORE_PASS**: ".keystore"/.jks" store password; default `V3ry1nS3cur3P5ssw0rd`
* **KEY_PASS**: Private key password; default `<ref:KEYSTORE_PASS>`

## HTTPS SSL Certificate via Let's Encrypt

### Prerequisites

1. A Linux machine connected to the Internet with ports 443 and 80 open
1. A domain/subdomain name pointing to this machine's IP address. (e.g., drawio.example.com)

### Method

1. Using kamalyes/drawio docker image, run the following command
`docker run -it -m1g -e LETS_ENCRYPT_ENABLED=true -e PUBLIC_DNS=drawio.example.com --rm --name="draw" -p 8080:80 -p 8443:443 kamalyes/drawio`
Notice that mapping port 80 to container's port 80 allows certbot to work in stand-alone mode. Mapping port 443 to container's port 8443 allows the container tomcat to serve https requests directly.

## Changing draw.io configuration

Configuration is managed by `DRAWIO_*` environment variables. For a list of these variables, check the `docker-entrypoint.sh` file

## Reference

* <https://github.com/jgraph/drawio>
