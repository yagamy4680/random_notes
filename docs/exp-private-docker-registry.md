---
title: Understand Docker Registry
layout: post
tags: ['docker','private registry', 'registry']
---

The article is my personal study to understand the storage layout of Docker's volumes and [Docker Registry](https://github.com/dotcloud/docker-registry), refer to following articles:

  - [Share Directories via Volumes](http://docs.docker.io/en/latest/use/working_with_volumes/)
  - [Private Repositories](http://docs.docker.io/en/latest/use/workingwithrepository/#private-repositories)
  - [docker-registry](https://github.com/dotcloud/docker-registry)
  - [samalba/docker-registry](https://index.docker.io/u/samalba/docker-registry/)

### Experiments

Create the shared data volume with container whose name is `DATA`. The data volume is located at `/tmp/registry`:
```bash
$ docker run -v /tmp/registry -name DATA busybox true
```

Run the built docker-registry `samalba/docker-registry` from Index server, and link the container to `DATA` container to share data volumes:
```bash
$ docker run -d -p 5000:5000 -volumes-from DATA samalba/docker-registry
```

The docker registry server is running at localhost:5000.

Then, let's create a simple nodejs container, and push it to our docker registry server. Here is the Dockerfile for nodejs image:

```text
# Pull base image.
FROM dockerfile/ubuntu

# Install Node.js
RUN apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:chris-lea/node.js
RUN apt-get update
RUN apt-get install -y nodejs

# Append to $PATH variable.
RUN echo '\n# Node.js\nexport PATH="node_modules/.bin:$PATH"' >> /root/.bash_profile
```

Build the nodejs image:
```bash
# Build
$ docker build -t yagamy/nodejs .

# Tag it with docker registry server
$ docker tag yagamy/nodejs localhost:5000/nodejs
```

Push the nodejs image onto Docker Registry server:
```bash
$ docker push localhost:5000/nodejs
```

After above step, let's observe the directory structure changes on docker registry server:
```bash
$ docker run -volumes-from DATA busybox sh -c "find /tmp/registry"
```

The directory structure is listed as follows:

```text
/tmp
  /registry
    /repositories
      /library
        /nodejs
          /_index_images
          /taglatest_json
          /json
          /tag_latest
    /images
      /3c089cb4358117fbb7592242eb0395c35b49b4304cce7d6db9fef3d4711e1ba3
        /layer
        /_checksum
        /json
        /ancestry
        /_files
      /6aaafedb78a7064b7e45b43c8edc25f973f03d029be41ebebfcea47e047301fa
      /662c8de3e84779b230d5d3f7c1816f607e3ced5f5e92ad0cc38b3e22a50b72b9
      /f9bc51d0ff19b3856c7b77165b3817055f4a9bcaebb4486e56bf9cc931c679f5
      /d85d887f973edf453f5fd4194fea9dcbe29c54e73959da9e1227a8b6acb3a6dc
      /c9d3c581211bffc48ade5b26a1f8a5dea1aa2e52af1d479dc5e5c65a07d26148
      /5fb268a4f34dfc725f3de427ec74a6828328afef00fab903837c4584890cf8a9
      /27cf784147099545
      /4daddba65a341832202371154ed60c8407f658b2cf3a40b9c1ee06c8b0f62dfa
      /b750fe79269d2ec9a3c593ef05b4332b1d1a02a62b4accb2c21d589ff2f5f2dc
      /0ff539f46db8539892877dc27a3eda2e9488f4d602ffa1520caeb848f9d5fe32
      /777d184a164376683d7ac4aa655940e9239a912791fe78bdbd0b13ae972bfd56
      /185edb5d9f53ea226b059bcedc7cbb39b0717e76f30871bd62d88b8e9f3a753e
      /42b7b56044800475f4cdffe6ec4896ffbb3ddc7eb145b5a3e443014f99767cc9
      /...
```

Assuming the docker registry server's ip address is 10.20.0.100, then, let's run the nodejs image on another Docker host machine (10.20.0.199):

```bash
# Pull nodejs image
$ docker pull 10.20.0.100:5000/nodejs
...

# Run the nodejs
$ docker run -t -i 10.20.0.100:5000/nodejs sh -c "node -e 'console.log(process.version)'"
v0.10.26
```
