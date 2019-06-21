# Getting Started using Docker

Right now, the easiest way to get started with the project is by using [docker](https://www.docker.com/).
More specifically, [docker-compose](https://docs.docker.com/compose/).
Each system can be run independently, but is much easier to do via the composition.
In order to get started with the docker-compose set up, you only need to clone this project.

**NOTE: The docker images for ecosystem are currently only produced for Linux.**
**Binaries are available for Linux, OSX, and Windows.**

## Running as a Standalone System

The system can be configured to run as a standalone system.
By default, the system defaults to using a SQLite3 database.

```
$ git clone git@github.com:deps-cloud/deps-cloud-project.git
$ cd deps-cloud-project/docker
$ docker-compose up -d
```

Once everything is up and running, you can test `rds` by curling the `/v1/repositories` endpoint.

```
$ curl -s http://localhost:8080/v1/repositories | jq
{
  "repositories": [
    "https://github.com/deps-cloud/gateway.git",
    "https://github.com/deps-cloud/rds.git",
    "https://github.com/deps-cloud/des.git",
    "https://github.com/deps-cloud/dts.git",
    "https://github.com/deps-cloud/deps-cloud-project.git",
    "https://github.com/deps-cloud/dis.git",
    "https://github.com/deps-cloud/base-image.git",
    "https://github.com/deps-cloud/gitfs.git",
    "https://github.com/deps-cloud/deps.cloud.git"
  ]
}
```

You can then pull the modules managed by a given repository using the `/v1/dependencies/managed` endpoint.

```
$ curl -s "http://localhost:8080/v1/dependencies/managed?url=https%3A%2F%2Fgithub.com%2Fdeps-cloud%2Frds.git" | jq
{
  "url": "https://github.com/deps-cloud/rds.git",
  "managed": [
    {
      "language": "go",
      "organization": "github.com",
      "module": "deps-cloud/rds"
    },
    {
      "language": "node",
      "organization": "deps-cloud",
      "module": "rds"
    }
  ]
}
```

This information can be used to look up consumers of the library using the `/v1/dependencies` endpoint.

```
$ curl -s "http://localhost:8080/v1/dependencies?language=go&organization=github.com&module=deps-cloud%2Frds" | jq
{
  "result": {
    "dependency": {
      "language": "go",
      "organization": "github.com",
      "module": "deps-cloud/finch"
    }
  }
}
{
  "result": {
    "dependency": {
      "language": "go",
      "organization": "github.com",
      "module": "deps-cloud/gitfs"
    }
  }
}
{
  "result": {
    "dependency": {
      "language": "go",
      "organization": "github.com",
      "module": "deps-cloud/dis"
    }
  }
}
{
  "result": {
    "dependency": {
      "language": "go",
      "organization": "github.com",
      "module": "deps-cloud/gateway"
    }
  }
}
```

## Swapping SQLite3 for MySQL

The storage layer for the tracking service supports both SQLite and MySQL.
The `docker-compose.mysql.yaml` file contains configuration demonstrating how to back the system with a MySQL database.
If you're going to try running this system, you will first need to bring up the storage tier.
Once MySQL starts, you can bring up the remaining processes.

```
$ docker-compose -f docker-compose.mysql.yaml up -d mysql

    ## wait for mysql to complete startup before bringing up the rest ##

$ docker-compose -f docker-compose.mysql.yaml up -d
```

When configured with MySQL, `dts` can be scaled horizontally.

## Swapping HTTP for SSH

The `docker-compose.ssh.yaml` file demonstrates how an SSH key can be used in place of HTTP cloning.
First, `rds` needs to be configured to return the SSH url's instead of the HTTP ones.
Then, `dis` needs access to the known_hosts and an authorized SSH key.
We currently do not support a password for the SSH key.
To generate one, simply run `ssh-keygen -b 2048 -t rsa -f <output>` on command line.
The private key is what will be passed into the indexer while the public key is uploaded to your integration.
