# gitfs

`gitfs` is probably the last component.
While it's not used in the critical path of the indexing pipeline at this time, it could be.
This project provides a FUSE file system that abstracts away the need to clone any repository.
It parses git URL's and builds them into a directory tree.
When you `cd` into a directory representing a git repository, it will shallowly clone the project on the fly.

This system is primarily focused on improving the automation experience by removing the need for engineers to clone every project they are patching.

## Resources

* [Golang](https://golang.org/)
* [GitHub Repository](https://github.com/deps-cloud/gitfs)
* [Travis CI/CD Job](https://travis-ci.com/deps-cloud/gitfs)
