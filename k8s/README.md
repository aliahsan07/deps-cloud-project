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
You can get the `NodePort` from the service ports.

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

The project also supports a high availability deployment using a replicated MySQL.
Currently there are two processes that cannot be horizontally scaled: `rds` and `dis`.
`rds` (repository discovery service) keeps an in memory map of repositories collected from different sources.
`dis` (dependency indexing service) reads from `rds` and distributes work among the backend systems.
These two systems will likely be changing in the next set of iterations.

In order to get started with the HA configuration, your Kubernetes cluster must be configured with either a [`StorageClass`](https://kubernetes.io/docs/concepts/storage/storage-classes/) or [`PersistentVolume`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) that meets MySQL's [persistent volume claim](https://github.com/deps-cloud/deps-cloud-project/blob/master/k8s/ha/mysql.yaml#L185-L192).
Most cloud providers come with a pre-configured storage class for their ecosystem.
If you're trying out the HA deployment locally in something like `minikube`, you can configure a `StorageClass` using local disk.

```
$ kubectl apply -f https://raw.githubusercontent.com/deps-cloud/deps-cloud-project/master/k8s/local-storage.yaml
```

Once you've ensured that your cluster has a configured `StorageClass`, you can apply the ha configuration.

```
$ git clone git@github.com:deps-cloud/deps-cloud-project.git
$ cd deps-cloud-project/k8s
$ kubectl apply -f ha/
```

It will take a minute or so for the whole system to boot, but once it does you'll be able to see each pod running.

```
$ kubectl get pods -n depscloud-system
NAME                       READY   STATUS    RESTARTS   AGE
des-6db6b5fc8c-22vjl       1/1     Running   0          90s
des-6db6b5fc8c-5pjth       1/1     Running   0          90s
des-6db6b5fc8c-bkwdk       1/1     Running   0          90s
des-6db6b5fc8c-nwx85       1/1     Running   0          90s
des-6db6b5fc8c-tgjwp       1/1     Running   0          90s
dis-7bb99cf676-gxll2       1/1     Running   0          91s
dts-8486b85447-p7wx6       1/1     Running   0          91s
dts-8486b85447-whkpr       1/1     Running   1          91s
dts-8486b85447-zpk5w       1/1     Running   1          91s
gateway-675d66f486-n254c   1/1     Running   0          91s
gateway-675d66f486-w52jk   1/1     Running   0          91s
gateway-675d66f486-wchq4   1/1     Running   0          91s
mysql-0                    2/2     Running   0          90s
mysql-1                    2/2     Running   0          61s
mysql-2                    2/2     Running   0          36s
rds-7f6fdc879c-krjxl       1/1     Running   0          90s
```

In the event that MySQL didn't come up, it's likely that you do not have a `StorageClass` configured for your cluster.

Similar to the standalone instructions above, you can grab either the LoadBalancer IP or NodePort.
When running in `minikube`, your `LoadBalancer` will never be assigned an External-IP.
In this case, just use the NodePort in combination with your `minikube ip` address.

```
$ kubectl get services -n depscloud-system gateway
NAME      TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
gateway   LoadBalancer   10.109.56.95   <pending>     8080:30621/TCP   21m
```
