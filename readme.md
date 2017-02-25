# How to use

## Make a Dockerfile
```
FROM ubuntu:latest
MAINTAINER Jan Sanchez <joejansanchez@gmail.com>

# Setting Enviroment variables
ENV NODE_VERSION 6.9.5
ENV NODE_ARCH x64
ENV TMP /tmp
ENV NODE_FILEPATH node-v$NODE_VERSION-linux-$NODE_ARCH

# Udpating and Installing dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    xz-utils \
    openssl \
    && rm -rf /var/lib/apt/lists/*

# Install Nodejs
RUN curl -SL https://nodejs.org/dist/v$NODE_VERSION/$NODE_FILEPATH.tar.xz -o $TMP/$NODE_FILEPATH.tar.xz \
    && cd $TMP/ && tar -xJf $NODE_FILEPATH.tar.xz && rm $NODE_FILEPATH.tar.xz \
    && mv $NODE_FILEPATH /opt/node \
    && ln -sf /opt/node/bin/node /usr/bin/node \
    && ln -sf /opt/node/bin/npm /usr/bin/npm

# Install the latest version of Yarn
RUN curl -SL https://yarnpkg.com/latest.tar.gz -o $TMP/latest.tar.gz \
    && cd $TMP/ && tar -zxf latest.tar.gz && rm latest.tar.gz \
    && mv $TMP/dist /opt/yarn \
    && ln -sf /opt/yarn/bin/yarn /usr/bin/yarn
```

## Build docker image of Ubuntu + Node
```
docker build -t jansanchez/ubuntu-node-yarn .
```

## Run docker image of Ubuntu + Node in a container
```
docker run -it -d jansanchez/ubuntu-node-yarn
```

## To go inside the last created container
```
docker exec -it $(docker ps | grep "node" | cut -c1-12) /bin/bash
```

## Kill the last created container
```
docker kill $(docker ps | grep "node" | cut -c1-12)
```

# Other Commands


## list docker images
```
docker images
```

## Build an image
```
docker build -t {newImageName} .
```

## Run a Docker image
```
docker run -p {clientPort}:{serverPort} -d {imageName}
```

## Remove a container
```
docker rm {containerName}
```

## Remove an docker image
```
docker rmi {imageName}
```

## List docker containers
```
docker ps
```

## To go inside the container
```
docker exec -it {containerID} /bin/bash
```

## Kill one or more running containers
```
docker kill {containerID}
```
