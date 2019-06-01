# Dependency Tracker

Dependency tracking is done via a knowledge graph.
The Dependency Tracker Service (`dts` for short) models the dependency graph in a SQL database.
The data store was loosely modeled after Dropbox's edgestore system, but with a few modifications.
Due to the query pattern of this system and need to long term support, I intentionally did not choose a graph database.

## Resources

* [Golang](https://golang.org/)
* [GitHub Repository](https://github.com/deps-cloud/dts)
* [Travis CI/CD Job](https://travis-ci.com/deps-cloud/dts)
* [Dropbox's Edgestore](https://www.youtube.com/watch?v=VZ-zJEWi-Vo)
