# Getting Started using Kubernetes

Kubernetes is now a supported method of deployment.

## Standalone Deployment

This deployment is not recommended for production uses.
It contains a single instance deployment for each component of the system.
The docs below assume you are running this on `minikube` for local testing.

```
$ git clone git@github.com:deps-cloud/deps-cloud-project.git
$ cd deps-cloud-project/k8s
$ kubectl apply -f standalone/
```

The default configuration sets up a `NodePort` for the `gateway` process to facilitate communication with backend systems.
You can get the NodePort from the service ports.

```
$ kubectl get services -n depscloud-system gateway
NAME      TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
gateway   NodePort   10.97.33.189   <none>        8080:31696/TCP   38s
```

In production, this can be swapped for a `LoadBalancer`.
With this NodePort, you can now curl the `/v1/repositories` endpoint.

```
$ curl -s "http://$(minikube ip):31696/v1/repositories" | jq
{
  "repositories": [
    "https://github.com/deps-cloud/deps.cloud.git",
    "https://github.com/deps-cloud/deps-cloud-project.git",
    "https://github.com/deps-cloud/dis.git",
    "https://github.com/deps-cloud/gateway.git",
    "https://github.com/deps-cloud/gitfs.git",
    "https://github.com/deps-cloud/dts.git",
    "https://github.com/deps-cloud/base-image.git",
    "https://github.com/deps-cloud/rds.git",
    "https://github.com/deps-cloud/des.git"
  ]
}
```

You can then pull the modules managed by a given repository using the `/v1/dependencies/managed` endpoint.

```
$ curl -s "http://$(minikube ip):31696/v1/dependencies/managed?url=https%3A%2F%2Fgithub.com%2Fdeps-cloud%2Frds.git" | jq
{
  "url": "https://github.com/deps-cloud/rds.git",
  "managed": [
    {
      "language": "golang",
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
$ curl -s "http://$(minikube ip):31696/v1/dependencies?language=golang&organization=github.com&module=deps-cloud%2Frds" | jq
{
  "result": {
    "dependency": {
      "language": "golang",
      "organization": "github.com",
      "module": "deps-cloud/finch"
    }
  }
}
{
  "result": {
    "dependency": {
      "language": "golang",
      "organization": "github.com",
      "module": "deps-cloud/gitfs"
    }
  }
}
{
  "result": {
    "dependency": {
      "language": "golang",
      "organization": "github.com",
      "module": "deps-cloud/dis"
    }
  }
}
{
  "result": {
    "dependency": {
      "language": "golang",
      "organization": "github.com",
      "module": "deps-cloud/gateway"
    }
  }
}
```

## High Availability Deployment

```
$ git clone git@github.com:deps-cloud/deps-cloud-project.git
$ cd deps-cloud-project/k8s
$ kubectl apply -f ha/
```
