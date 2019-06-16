# deps.cloud

deps.cloud is a set of systems that parses, indexes, and stores dependencies between libraries.
The information is then served over gRPC or REST for easy consumption by automated processes.
This project was largely based on a system I wrote with little concern for it's longevity.
As a result, that project wasn't easy to:

1. Open source for accessibility
2. Support languages other than Java
3. Decouple functionality to improve scalability

## Project Space

This space will be used to track end user feature requests as well as general project interest.
Issues should typically speak to a general capability that will require work across the various project dependencies.

## Overview

A complete overview of the system's architecture can be found [here](systems/README.md).
The system is currently comprised of 5 parts:

* The [Repository Discovery Service](systems/repository-discovery.md) (`rds`) abstracts away repository providers (like GitHub, GitLab, or BitBucket).
* The [Dependency Extraction Service](systems/dependency-extractor.md) (`des`) consumes repository files and returns the various management files it discovers.
* The [Dependency Tracking Service](systems/dependency-tracker.md) (`dts`) consumes dependency management files and stores them in a database.
* The [Dependency Indexing Service](systems/dependency-indexer.md) (`dis`) coordinates work with `rds`, `des`, and `dts`.
* [gitfs](systems/gitfs.md) provides a virtual file system to repositories discovered by `rds`.

## Getting Started

You can now start using this project by running the system via [Docker](docker) or [Kubernetes](k8s).
Our Kubernetes setup also includes an HA configuration out of box.
Two systems, `rds` and `dis` are not yet highly available.

Once you have the system up and running, you can learn how to configure it for your desired integrations by stopping by our [configuration](configuration) section.

### Longer Term

* A UI for being able to visualize and track dependencies on projects.
* Automation to aid in library upgrades and management.

## Help Contribute

I've been working on the project for over a year and have made slow and steady progress.
Having some folks who are similarly passionate about the project space and want to help push the project forward would be much appreciated.
If you're looking to help contribute to the project, you can reach out to me (@mjpitz) a few different ways.

* We have a [discord](https://discord.gg/dBXVkyP) channel where most of the contributor conversation occurs.
* We have a [Gitter](https://gitter.im/depscloud/community) for those who do not actively use discord.
* We have a [FindCollabs](https://findcollabs.com/project/GIOlcUiHE9XD2UVlxrNl) project to help draw interest from others looking to help contribute.

We will probably collapse onto a single chat system at some point.
Given the current project status, I figured having more means of communication is better than none.
If you'd like to help contribute, take a look at the opportunities below and reach out to the team to offer assistance.

See the [contributing](contributing) space for more details.

### As a backend engineer

Most of the backend to this system is written in [Golang](https://golang.org/).
This language was chosen for it's easy integrations with repository providers like GitHub, GitLab, and BitBucket.
Previous implementations used Java and JavaScript, but fell short on a ease portability.
Golang expanded support for this project to Windows systems where previous implementations were tied to Unix systems.

The dependency extraction system is the one case where [Typescript]() was used instead of Golang.
This was an important design decision.
For details and rationale on why Typescript was chosen for this project, see the [Dependency Extractor](systems/dependency-extractor.md) documentation.

### As a front end engineer

The front end components of this project right now are rather minimal.
Most of the system exists in the backend and most of the functionality is there to start building a UI on top of it.
The `docker` directory can be used to get the core of the system up and running.

### As a graphic designer

There are a few different ways graphic designers can help with this project.
First, we need some good default project assets.
This includes logos, banners, icons, etc.
Second, the [landing site](https://deps.cloud) could use some good tlc.
The current page was assembled to get something up there and start attracting interest.
Finally, designers can help drive content that should be included on the project main page.

### As a technical writer

Aside from core programming work, I am looking for some technical writers to help contribute.
This includes, but is not limited to:
* Project API documentation
* Technical content for the landing site
* Blog posts with status updates

<!--
### As a community manager

This project was started from a previous project that @mjpitz worked on.
It draws inspiration from several of the mono-repository efforts taken at other companies while accepting that micro-repositories will always be something to consider.
A community manager can help keep up with some of the book-keeping and management of this project.
They should help ensure the project is progressing and maintain communication with the community at large.
Additionally, they can help validate this project and direction with other companies.
-->
