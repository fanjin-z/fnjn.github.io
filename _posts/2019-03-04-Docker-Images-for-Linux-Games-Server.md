layout: post
title:  "Docker Images for Linux Games Server"
date:   2019-03-04 00:00:00 -0800
categories: Linux-Games-Server, Docker
---

I created docker images for Counter-strike: Global Offensive, Counter-strike: Source, Counter-strike: 1.6 and Garry's Mod Linux dedicated servers. All these images are well tested and available on both Docker Hub and Github. Feel free to download!

## CS:GO
### Pull from DockerHub
```bash
docker pull fnjn/csgo:latest
docker run -d -p 27015:27015 csgo:latest
```

### Build from Dockerfile
```bash
git git@github.com:Fnjn/csgo-docker.git
cd csgo-docker
docker build . -t csgo:latest
docker run -d -p 27015:27015 csgo:latest
```
Checkout configuration info on [Fnjn/csgo-docker](https://github.com/Fnjn/csgo-docker)


## CS:S
### Pull from DockerHub
```bash
docker pull fnjn/css:latest
docker run -d -p 27015:27015 css:latest
```

### Build from Dockerfile
```bash
git git@github.com:Fnjn/css-docker.git
cd css-docker
docker build . -t css:latest
docker run -d -p 27015:27015 css:latest
```
Checkout configuration info on [Fnjn/css-docker](https://github.com/Fnjn/css-docker)


## CS 1.6
### Pull from DockerHub
```bash
docker pull fnjn/cs1.6:latest
docker run -d -p 27015:27015 cs1.6:latest
```

### Build from Dockerfile
```bash
git git@github.com:Fnjn/cs1.6-docker.git
cd cs1.6-docker
docker build . -t cs1.6:latest
docker run -d -p 27015:27015 cs1.6:latest
```
Checkout configuration info on [Fnjn/cs1.6-docker](https://github.com/Fnjn/cs1.6-docker)


## Garry's Mod
### Pull from DockerHub
```bash
docker pull fnjn/garrysmod:latest
docker run -d -p 27015:27015 garrysmod:latest
```

### Build from Dockerfile
```bash
git git@github.com:Fnjn/garrysMod-docker.git
cd garrysMod-docker
docker build . -t garrysmod:latest
docker run -d -p 27015:27015 garrysmod:latest
```
Checkout configuration info on [Fnjn/garrysMod-docker](https://github.com/Fnjn/garrysMod-docker)
