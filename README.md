Traccar in Docker
---

**Suntrack GPS Tracking System in Docker image.**

Official website: <https://www.suntrack.com.au>  
DockerHub image: <https://hub.docker.com/r/suntrackserver/suntrack> ![](https://img.shields.io/docker/stars/suntrackserver/suntrack) ![](https://img.shields.io/docker/pulls/suntrackserver/suntrack)  
Maintainer: [Dinith Herath](https://github.com/dinithherath)

## Available tags:
#### 6.X
- **6.6.s1-alpine**, **6-alpine**, **alpine**, **6.6.s1**, **6**, **latest** ![](https://img.shields.io/docker/image-size/suntrackserver/suntrack/6.1.s1-alpine)
- **6.6.s1-debian**, **6-debian**, **debian** ![](https://img.shields.io/docker/image-size/suntrackserver/suntrack/6.1.s1-debian)
- **6.6.s1-ubuntu**, **6-ubuntu**, **ubuntu** ![](https://img.shields.io/docker/image-size/suntrackserver/suntrack/6.1.s1-ubuntu)
- _..._
- _**6.0.s1**, **6.0.s1-alpine** / **6.0.s1-debian** / **6.0.s1-ubuntu**_
#### 5.X
- _**5.10**, **5.10-alpine** / **5.10-debian** / **5.10-ubuntu**_
- _..._
- _**5.0**, **5.0-alpine** / **5.0-debian**_
#### 4.X
- _**4.15**, **4.15-alpine** / **4.15-debian** / **4.15-ubuntu**_
- _..._
- _**4.0**, **4.0-alpine** / **4.0-debian**_
#### 3.X
- _**3.17**, **3.17-alpine** / **3.17-debian**_
- _**3.16**, **3.16-alpine** / **3.16-debian**_

## Available multi-platform images:
**Alpine based**: linux/amd64, linux/arm64  
**Debian based**: linux/amd64, linux/arm64  
**Ubuntu based**: linux/amd64, linux/arm64, linux/arm/v7

## Container create example:
1. **Create work directories:**
    ```bash
    mkdir -p /opt/traccar/logs
    ```

1. **Get default traccar.xml:**
    ```bash
    docker run \
    --rm \
    --entrypoint cat \
    traccar/traccar:latest \
    /opt/traccar/conf/traccar.xml > /opt/traccar/traccar.xml
    ```

1. **Edit traccar.xml:** <https://www.traccar.org/configuration-file/>

1. **Create container:**
    ```bash
    docker run \
    --name traccar \
    --hostname traccar \
    --detach --restart unless-stopped \
    --publish 80:8082 \
    --publish 5000-5150:5000-5150 \
    --publish 5000-5150:5000-5150/udp \
    --volume /opt/traccar/logs:/opt/traccar/logs:rw \
    --volume /opt/traccar/traccar.xml:/opt/traccar/conf/traccar.xml:ro \
    --volume /opt/traccar/data:/opt/traccar/data:rw \
    traccar/traccar:latest
    ```

## Database
The default when executing the above `docker run` command is an internal H2 database but this should only be for basic use.  

The **recommended solution** for production use is to link to an external MySQL database and update the configuration `.xml`-file according to the [Traccar MySQL documentation](https://www.traccar.org/mysql/) and using the `docker run` command as-is.

## Default JVM options:
- `-Xms1g`
- `-Xmx1g`
- `-Djava.net.preferIPv4Stack=true`
