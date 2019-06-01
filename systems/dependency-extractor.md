# Dependency Extractor

Dependency extraction is a core component of this system.
Due to the variability of dependency management files, I wanted this system be flexible to support different file types.
The Dependency Extraction Service (`des` for short) was written in Typescript due to it's ability to be optionally typed.
It provides static analysis where it can while allowing the parsing logic to be more flexible to the problem.

## Why Typescript?

Unlike it's other backend counterparts, the dependency extraction service was written in [Typescript](https://www.typescriptlang.org/).
Typescript in this case was an intentional design decision that came from an previous rewrite.
The systems original implementation was in Java and proved to be quite difficult to work with.
After rewriting the extraction logic in JavaScript, I liked the simplicity involved in parsing dependencies.
The downside about JavaScript was I lost static typing where I wanted static typing.
Enter Typescript.
After adding it to the project, I really started to enjoy being able to swap between statically typed variables and duck types when needed.

## Why a Service?

While the current implementation exists as a single service, I wanted the ability to freely swap the backend technologies.
I also wanted the ability to create a composition service that enables a polyglot backend for parsing management files.
Parsing `build.gradle` files outside of languages like Java can be difficult.
A longer term, more reliable solution would be to write a GradleDependencyExtractor that implements the gRPC API and encapsulates the logic for parsing Gradle files.
In the meantime, using a looser typed language like Typescript has allowed this system to iterate and grow.

## Resources

* [Typescript Language](https://www.typescriptlang.org/)
* [GitHub Repository](https://github.com/deps-cloud/des)
* [Travis CI/CD Job](https://travis-ci.com/deps-cloud/des)
