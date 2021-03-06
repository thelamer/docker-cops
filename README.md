[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)

The [LinuxServer.io](https://linuxserver.io) team brings you another container release featuring :-

 * regular and timely application updates
 * easy user mappings (PGID, PUID)
 * custom base image with s6 overlay
 * weekly base OS updates with common layers across the entire LinuxServer.io ecosystem to minimise space usage, down time and bandwidth
 * regular security updates

Find us at:
* [Discord](https://discord.gg/YWrKVTn) - realtime support / chat with the community and the team.
* [IRC](https://irc.linuxserver.io) - on freenode at `#linuxserver.io`. Our primary support channel is Discord.
* [Blog](https://blog.linuxserver.io) - all the things you can do with our containers including How-To guides, opinions and much more!

# [linuxserver/cops](https://github.com/linuxserver/docker-cops)
[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/cops.svg)](https://microbadger.com/images/linuxserver/cops "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/cops.svg)](https://microbadger.com/images/linuxserver/cops "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/cops.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/cops.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-cops/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-cops/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/cops/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/cops/latest/index.html)

[Cops](http://blog.slucas.fr/en/oss/calibre-opds-php-server) by Sébastien Lucas, stands for Calibre OPDS (and HTML) Php Server.

COPS links to your Calibre library database and allows downloading and emailing of books directly from a web browser and provides a OPDS feed to connect to your devices.

Changes in your Calibre library are reflected immediately in your COPS pages.

See : [COPS's home](http://blog.slucas.fr/en/oss/calibre-opds-php-server) for more details.

Don't forget to check the [Wiki](https://github.com/seblucas/cops/wiki).

## Why? (taken from the author's site)

In my opinion Calibre is a marvelous tool but is too big and has too much
dependencies to be used for its content server.

That's the main reason why I coded this OPDS server. I needed a simple
tool to be installed on a small server (Seagate Dockstar in my case).

I initially thought of Calibre2OPDS but as it generate static file no
search was possible.

Later I added an simple HTML catalog that should be usable on my Kobo.

So COPS's main advantages are :
 * No need for many dependencies.
 * No need for a lot of CPU or RAM.
 * Not much code.
 * Search is available.
 * With Dropbox / owncloud it's very easy to have an up to date OPDS server.
 * It was fun to code.

If you want to use the OPDS feed don't forget to specify feed.php at the end of your URL.


[![cops](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/cops-icon.png)](http://blog.slucas.fr/en/oss/calibre-opds-php-server)

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/cops` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v7-latest |


## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=cops \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -p 80:80 \
  -v <path to data>:/config \
  -v <path to data>:/books \
  --restart unless-stopped \
  linuxserver/cops
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  cops:
    image: linuxserver/cops
    container_name: cops
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - <path to data>:/config
      - <path to data>:/books
    ports:
      - 80:80
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 80` | WebUI |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London. |
| `-v /config` | COPS Application Data. |
| `-v /books` | Calibre metadata.db location. |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```


&nbsp;
## Application Setup

Unlike other implementations of COPS in a docker container,  the linuxserver version gives you access to `config_local.php` in `/config` to customise your install to suit your needs, including details of your email account etc to enable emailing of books, it also includes the dependencies required to directly view epub books in your browser.



## Support Info

* Shell access whilst the container is running: `docker exec -it cops /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f cops`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' cops`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/cops`

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (ie. nextcloud, plex), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.  
  
Below are the instructions for updating containers:  
  
### Via Docker Run/Create
* Update the image: `docker pull linuxserver/cops`
* Stop the running container: `docker stop cops`
* Delete the container: `docker rm cops`
* Recreate a new container with the same docker create parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* Start the new container: `docker start cops`
* You can also remove the old dangling images: `docker image prune`

### Via Taisun auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one shot:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock taisun/updater \
  --oneshot cops
  ```
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull cops`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d cops`
* You can also remove the old dangling images: `docker image prune`

## Versions

* **23.03.19:** - Switching to new Base images, shift to arm32v7 tag.
* **27.02.19:** - Upgrade packages during install to prevent mismatch with baseimage.
* **22.02.19:** - Rebasing to alpine 3.9.
* **14.01.19:** - Add multiarch and pipeline logic.
* **21.08.18:** - Rebase to alpine 3.8.
* **02.07.18:** - Add php7-ctype dependency.
* **08.01.18:** - Rebase to alpine 3.7.
* **25.05.17:** - Rebase to alpine 3.6.
* **03.04.17:** - Add composer packages, reduce layers.
* **02.04.17:** - Updated to version 1.1.0.
* **05.02.17:** - Updated to Alpine 3.5 & PHP7.
* **24.10.16:** - Updated to implement user based config.
* **24.10.16:** - Updated to version 1.0.1.
* **14.10.16:** - Add version layer information.
* **28.09.16:** - Add php5-zlib.
* **11.09.16:** - Add layer badges to README.
* **29.08.16:** - Add php5-opcache.
* **28.08.16:** - Add badges to README.
* **12.08.16:** - Initial Release.
