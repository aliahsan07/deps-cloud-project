# Dependency Tracker

Dependency tracking is done via a knowledge graph.
The system models the dependency graph in a SQL database.
The data store was modeled after the Dropbox's edgestore system, but with a few modifications.
Due to the query pattern of this system, I intentionally did not choose a graph database.
Additionally, there is tons of support for SQL databases in open source where Graph database support is a bit harder to find.

## Resources

* [GitHub Repository](https://github.com/deps-cloud/dts)
* [Travis CI/CD Job](https://travis-ci.com/deps-cloud/dts)
