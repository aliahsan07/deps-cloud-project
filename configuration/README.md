# Getting Started

This section provides guidance on how to configure the underlying system.
Keep in mind, how you update the configuration depends on your deployment strategy.
If you're using Docker, you will be changing the `rds.yaml` file.
If you're using Kubernetes, you will need to change the `rds-config` `ConfigMap`.

## Configuring Repository Discovery

Repository Discovery is responsible for the discovery of repositories from different sources.
These sources include systems like BitBucket, GitHub, and GitLab.
Configuring the system to read from these different sources is rather easy.
See the following configuration guides on how to configure the system for your corresponding provider.

* [Configuring for BitBucket](bitbucket.md)
* [Configuring for GitHub](github.md)
* [Configuring for GitLab](gitlab.md)
