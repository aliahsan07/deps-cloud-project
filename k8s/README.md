# Getting Started using Kubernetes

Kubernetes is now a supported method of deployment.
While we do not have an HA configuration right now, one will be coming.
Currently, we support running this as a standalone system.

## Running as a Standalone System

```
$ git clone git@github.com:deps-cloud/deps-cloud-project.git
$ cd deps-cloud-project/k8s
$ kubectl apply -f standalone/
```

## Swapping SQLite3 for MySQL

TBD
