# What Is Helm?

Successful projects have an identity. What they do is known and, often, clear. This helps everyone know where a project begins and ends. It is used to market a project so that consumers know what to expect. And, it helps maintainers know the scope of a project so they can say no to things that are out of scope.

## Helm, A Package Manager

A mental model I have for Kubernetes is as a cluster level operating system that has [hot swappable](https://en.wikipedia.org/wiki/Hot_swapping) hardware (i.e. nodes in the cluster).

Kubernetes is patterned after [Borg](https://research.google/pubs/pub43438/), the cluster manager for Google's [datacenter as a computer](https://books.google.com/books/about/The_Datacenter_as_a_Computer.html?id=b951DwAAQBAJ) setup.

Operating system users like to have package managers. Every Linux distribution has a package manager. macOS and Windows both have package managers from 3rd parties. A gap needed to be filled and users are fond of the solutions that have filled that gap (e.g., [Homebrew](https://brew.sh/)).

It's worth taking a moment to understand why end-users like package managers. Take running PostgreSQL on Linux as an example. Using a package manager someone can install PostgreSQL with a simple command. If someone wanted to do it by hand they would have a lot more work. By hand someone would need to know where to get the binaries from and where to put them on the operating system. If there are shared libraries they would need to know about those and where to put them. Configuration files would need to be written to the right place with the right content. And, PostgreSQL would need to be setup to run automatically. To install PostgreSQL one would need to have expertise in their Linux distribution of choice and PostgreSQL itself.

People who use package managers usually have other things they want to get done. For example, they are running their own software and have dependencies they need to use. They don't want to spend the time to become experts in all of their dependencies. Instead, they just want to install and run it.

Package managers provide a nice interface between those with different expertise to help each other out. Someone who creates a package will have expertise in the software and the platform. The consumer of the software doesn't need to have any expertise in either of those. Instead, they can get the software and more on to what they care about quickly.

_Helm is the package manager for Kubernetes._ It provides a way to package software software to be easily consumed. Consumers don't need to know extensive details about Kubernetes or the application they are running in order to be successful with it.

## Roles & Personas

To help the Helm maintainer know their target audience and prioritize them accordingly, there is [a prioritized list of roles](https://github.com/helm/community/blob/main/user-profiles.md) (a.k.a. profiles). It's important to know your users and have a mental model for them. This list has the roles of:

1. Application Operator - The person who installs and uses an application in Kubernetes (e.g., MariaDB).
2. Application Distributor - The person who packages up an application for Application Operators to consume and use.
3. Application Developer - Those who write applications (e.g., the developers of MariaDB).
4. Supporting Tool Developer - People developing supporting tools for Helm or on top of Helm.
5. Helm Developer - Anyone developing Helm or maintaining it.

Notice, the Helm maintainers prioritize themselves last when compared to others involved in the project. They also prioritize those who consume and use package above anyone creating an application or packaging it. Making sure end users who just want to use the software are successful is a high priority.

A single person can perform one or more of the roles. It really depends on the type of organization they are in and the size of it.

One role is marked as out of scope for Helm. That is the cluster operator. Cluster operators have many tools to help them manage their cluster.

### Example Applying The Priorities

While the maintainer were designing Helm v3 we interviewed application operators and cluster operators providing their clusters to application operators to use. We learned about constraints they had (especially around [authorization and permissions](https://kubernetes.io/docs/reference/access-authn-authz/authorization/)).

This led to some design decisions including:

- Storing release information in Secrets by default without needing to do anything. Secrets are available in all Kubernetes clusters. Many application operators don't have permission to install Cluster Resource Definitions (CRDs) to extend the API. Secrets didn't suffer from this limitation. This decision was around knowing the constraints of the highest priority role.
- The Secrets are versioned within a type. This makes it easier for those building supporting tools to work with Helm secrets when they need to.
- The ability to provide a different mechanism to store release information. This enables supporting tool developers and those with more advanced use cases. They can break away from the default if they want.

## Handling Out Of Scope Requests

When you know what a project is and does it becomes easier to identify what is out of scope and to say no to it. Scope creep can kill a project. There become too much to maintain, too much complexity, or no clear scope.

The Helm projects has gotten many requests over time. Some of these are out of scope for Helm. I've found a few good ways to handle these.

- There are other projects in the cloud native stacks used to manage workloads. If a request fits into the space for one or more of those projects, we can point people to those projects. Those ideas may already exist in one of those projects or may be a potential addition.
- When there is no other place the idea fits it's an opportunity to encourage the creation of a new project to experiment with the idea.
- Sometimes the idea introduces a pattern that has downfalls in a cloud native environment. This is an opportunity to explain the downfalls and other patterns that provide a better solution for the underlying problem.

No matter how we reject an idea, it's important to treat the person who brought the idea kindly. They thought it was an idea worth sharing and, I think, we should have respect for that. Even when we don't accept it.

## Client & SDK

There are two parts to Helm that are important to think about.

First, there is the Helm Client (a.k.a. the Helm CLI). This is the `helm` command someone will use in a terminal, a script, or is executed by another program.

Second, there is the Helm SDK which is written in Go. This is the `/pkg` directory in the Helm source. The Helm SDK is meant for software developers to use when building their own applications on top of Helm.

The Helm SDK includes both package for working with specific components (e.g., charts) and application business logic (e.g., for installing a Helm chart as a release). In most cases the Helm client calls to the business logic in the SDK and serves as an example for those who want to build on top of the Helm SDK.
