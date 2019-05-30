# deps.cloud

deps.cloud is a set of systems that parses, indexes, and stores dependencies between libraries.
The information is then served over gRPC or REST for easy consumption by automated processes.
This project was largely based on a system I wrote with little concern for it's longevity.
As a result, that project wasn't easy to:

1. Open source for accessibility
2. Support languages other than Java
3. Decouple functionality to improve scalability

## Overview

* The [Repository Discovery Service](https://github.com/deps-cloud/rds) (`rds`) abstracts away repository providers (like GitHub, GitLab, or BitBucket).
* The [Dependency Extraction Service](https://github.com/deps-cloud/des) (`des`) consumes repository files and returns the various management files it discovers.
* The [Dependency Tracking Service](https://github.com/deps-cloud/dts) (`dts`) consumes dependency management files and stores them in a database.
* The [Dependency Indexing Service](https://github.com/deps-cloud/dis) (`dis`) pulls repositories from `rds`, uses `des` to extract dependencies, then stores the results in `dts`.
* [gitfs](https://github.com/deps-cloud/gitfs) is a FUSE file system that provides a virtual file system over shallowly cloned repositories.

### Coming Soon

* A process responsible for consuming repositories from `rds`, streaming their data to `des` for extraction, then storing the resulting data in `dts`.
* A `docker-compose.yaml` file for a standalone setup, facilitating adoption
* [Kubernetes](https://kubernetes.io) configuration for easy high availability deployment.

### Longer Term

* A UI for being able to visualize and track dependencies on projects.
* Project dependency intelligence.
* Automation to aid in library management.
* Provide system as a paid service for those who do not want to run it themselves.

## Project Space

This space will be used to track end user feature requests as well as general project interest.

## Interested in the project?

Follow this project by starring this repository.
I plan on sending out some periodic updates as components become more readily available.

## Looking for help

I am actively looking for help across the various projects in this system.
I could use some help with engineering, technical documentation, front end construction, and general ideation.
If you're interested, please reach out in the form of a GitHub issue on this project.
Let me know what you're interested in helping with.

Thanks!
