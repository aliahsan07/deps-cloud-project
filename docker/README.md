# Getting Started using Docker

Right now, the easiest way to get started with the project is by using [docker](https://www.docker.com/).
More specifically, [docker-compose](https://docs.docker.com/compose/).
Each system can be run independently, but is much easier to do via the composition.
In order to get started with the docker-compose set up, you only need to clone this project.

**NOTE: The docker images for ecosystem are currently only produced for Linux. Binaries are available for Linux, OSX, and Windows.**

```
$ git clone git@github.com:deps-cloud/deps-cloud-project.git
$ cd deps-cloud-project/docker
$ docker-compose up
Starting docker_rds_1 ... done
Starting docker_dts_1 ... done
Starting docker_des_1 ... done
Recreating docker_dis_1 ... done
Attaching to docker_dts_1, docker_des_1, docker_rds_1, docker_dis_1
dts_1  | time="2019-05-31T03:28:11Z" level=info msg="[main] starting gRPC on :8090"
rds_1  | time="2019-05-31T03:28:11Z" level=info msg="[main] starting gRPC on :8090"
des_1  | 
des_1  | > @deps-cloud/des@0.1.4 start /app
des_1  | > node lib/main.js
des_1  | 
rds_1  | time="2019-05-31T03:28:11Z" level=info msg="[service.rds] loading repositories"
rds_1  | time="2019-05-31T03:28:11Z" level=info msg="[remotes.github] processing repositories for organization: deps-cloud"
des_1  | [2019-05-31T03:28:13.072] [INFO] default - [main] starting gRPC on :8090
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/rds.git] cloning repository"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/dis.git] cloning repository"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/gitfs.git] cloning repository"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/deps-cloud-project.git] cloning repository"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/base-image.git] cloning repository"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/base-image.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/base-image.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/base-image.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:13Z" level=info msg="[https://github.com/deps-cloud/des.git] cloning repository"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps-cloud-project.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps-cloud-project.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dis.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps-cloud-project.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dts.git] cloning repository"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dis.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dis.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps.cloud.git] cloning repository"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/gitfs.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/gitfs.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/gitfs.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/rds.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/rds.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/rds.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dts.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dts.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/dts.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/des.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/des.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps.cloud.git] matching dependency files"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/des.git] storing dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps.cloud.git] extracting dependencies"
dis_1  | time="2019-05-31T03:28:14Z" level=info msg="[https://github.com/deps-cloud/deps.cloud.git] storing dependencies"
```
