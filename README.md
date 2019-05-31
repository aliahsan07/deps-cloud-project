# deps.cloud

deps.cloud is a set of systems that parses, indexes, and stores dependencies between libraries.
The information is then served over gRPC or REST for easy consumption by automated processes.
This project was largely based on a system I wrote with little concern for it's longevity.
As a result, that project wasn't easy to:

1. Open source for accessibility
2. Support languages other than Java
3. Decouple functionality to improve scalability

## Overview

A complete overview of the system's architecture can be found [here](systems/README.md).
The system is currently comprised of 5 parts:

* The [Repository Discovery Service](systems/repository-discovery.md) (`rds`) abstracts away repository providers (like GitHub, GitLab, or BitBucket).
* The [Dependency Extraction Service](systems/dependency-extractor.md) (`des`) consumes repository files and returns the various management files it discovers.
* The [Dependency Tracking Service](systems/dependency-tracker.md) (`dts`) consumes dependency management files and stores them in a database.
* The [Dependency Indexing Service](systems/dependency-indexer.md) (`dis`) coordinates work with `rds`, `des`, and `dts`.
* [gitfs](systems/gitfs.md) provides a virtual file system to repositories discovered by `rds`.

## Getting Started

You can now start using this project by running the system via docker.
For more information on how to, check out our [docker](docker) section of the project.

### Coming Soon

* [Kubernetes](https://kubernetes.io) configuration for easy high availability deployment.

### Longer Term

* A UI for being able to visualize and track dependencies on projects.
* Automation to aid in library management.

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
