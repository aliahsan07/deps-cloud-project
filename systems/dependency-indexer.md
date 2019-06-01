# Dependency Indexer

The Dependency Indexer Service (`dis` for short) handles all the business logic of the system.
It watches `rds` for new repositories and clones them for indexing.
It calls `des` with the directory tree to find all matching files and extracts the dependencies from all matching.
The results are then stored in `dts` where the information can be readily queried.

## Resources

* [Golang](https://golang.org/)
* [GitHub Repository](https://github.com/deps-cloud/dis)
* [Travis CI/CD Job](https://travis-ci.com/deps-cloud/dis)
