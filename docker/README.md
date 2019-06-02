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

You can also test `dts` by curling the `/v1/dependencies` endpoint.

```
$ curl -s "http://localhost:8080/v1/dependencies?language=golang&organization=github.com&module=deps-cloud%2Frds"
{"result":{"dependency":{"organization":"github.com","module":"deps-cloud/finch"}}}
{"result":{"dependency":{"organization":"github.com","module":"deps-cloud/gitfs"}}}
{"result":{"dependency":{"organization":"github.com","module":"deps-cloud/dis"}}}
{"result":{"dependency":{"organization":"github.com","module":"deps-cloud/gateway"}}}
```

Because this endpoint is implemented as a stream, you can pipe it through to other commands, like `jq`.

```
$ curl -s "http://localhost:8080/v1/dependencies?language=golang&organization=github.com&module=deps-cloud%2Frds" | jq
{
  "result": {
    "dependency": {
      "organization": "github.com",
      "module": "deps-cloud/finch"
    }
  }
}
{
  "result": {
    "dependency": {
      "organization": "github.com",
      "module": "deps-cloud/gitfs"
    }
  }
}
{
  "result": {
    "dependency": {
      "organization": "github.com",
      "module": "deps-cloud/dis"
    }
  }
}
{
  "result": {
    "dependency": {
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
