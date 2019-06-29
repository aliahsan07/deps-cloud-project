# Getting Started

## Overview

## Quickstart (Using Docker)

In this quickstart, we'll use [docker-compose](https://docs.docker.com/compose) to create our demo infrastructure.

To get started quickly, you can clone the [deps-cloud-project](https://github.com/deps-cloud/deps-cloud-project) repository and leverage the configuration under the `user_guides/docker` directory.

### 1 - Launch deps.cloud

To launch the demo infrastructure, simply run the following:

```
$ docker-compose up -d
Creating docker_des_1 ... done
Creating docker_dts_1 ... done
Creating docker_dis_1     ... done
Creating docker_gateway_1 ... done
```

**That's it!**

The deps.cloud demo infrastructure is up and running.
You can test it out by opening any of the following endpoints (browser or curl).

* [What sources have been Indexed?](http://localhost:8080/v1alpha/sources)
* [What modules are produced by this repository?](http://localhost:8080/v1alpha/modules/managed?url=https%3A%2F%2Fgithub.com%2Fdeps-cloud%2Fdes.git)
* [What modules do I depend on and what version?](http://localhost:8080/v1alpha/graph/go/dependencies?organization=github.com&module=deps-cloud%2Fdes)
* [What modules depend on me and what version?](http://localhost:8080/v1alpha/graph/go/dependents?organization=github.com&module=deps-cloud%2Fdes)
* [What repository produces this module?](http://localhost:8080/v1alpha/modules/source?organization=github.com&module=deps-cloud%2Fdes&language=go)

### 2 - Configure Your Provider

Once the demo infrastructure is up and running, you can easily modify the `rds.yaml` file to include more accounts to index.
For more information on how to configure each integration, see the [configuration](../configuration/README.md) section.

```
accounts:
- github:
    strategy: HTTP
    organizations:
    - deps-cloud
- bitbucket:
    teams:
    - atlassian
    strategy: HTTP
- gitlab:
    groups:
    - nvidia
    strategy: HTTP
```

The configuration will not be automatically reloaded.
All processes leveraging the configuration will need to be restarted to pick up the new version.

### 3 - Rerun Indexer

Once the configuration has been updated to include the new bits you want to index, you can restart the indexer process. Doing so will cause the indexer to pick up changes to the `rds.yaml` file and index the new repositories.

```
$ docker-compose restart dis
Restarting docker_dis_1 ... done
```
