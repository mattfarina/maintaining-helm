# Reviewing PRs

One of the most important jobs of a maintainer is to review and merge PRs. There are some very practical things a maintainer can do to help this process along.

## Number Of Approvers

One of the important things to look at for Helm is the number of approvers a pull request needs before being merged. There are some pretty simple rules for this.

1. For an extra small change (i.e. labled `size/xs`) only one maintainer needs to approve the change. Unless the reviewing maintainer wants to have more reviews. These changes are typically very simple.
2. For a pull request written by a maintainer, only one other maintainer needs to approve prior to merging. The idea is to have 2 maintainers be understand the change. In this cause it's the author and approver.
3. Any pull request from the community that are small or larger, there needs to be two approvals from maintainers.

This is documented, in more detail, in the [contributing guide](https://github.com/helm/helm/blob/main/CONTRIBUTING.md).

## Who Merges A Pull Request

This may seem obvious but there are two different situations.

- For a pull request from a maintainer, it is the maintainer who wrote the pull request who should merge it. Once it has enough approvals, the pull request is left to the maintaining author to merge.
- Pull requests from the community are merged by the maintainer who gave it the final required approval.

## Common Things To Catch In A Review

There are many common things that are easy to catch in a review. This is an incomplete list and should be considered a start.

### Public API Changes

Helm follows semantic versioning and cannot break the public API until the next major version of Helm is released.

There are three places to look for changes to the public API.

#### The Go API

An easy way to do this is to look for changes to functions that start with capital letters. If the signature changes there is a problem.

The way to handle this in Helm is to create a new function and deprecate the old one. There are examples of this within the Helm codebase already. When a new major version of Helm is released, the deprecated functions can be cleaned up.

A second place to look at the code APIs is with Interfaces. According to the Go team, changing an Interface is a breaking change. So, once an Interface is released for Helm it cannot be change until there is a new Helm major version release. Instead, new interfaces need to be created and type switching must be used. An [example of handling new interfaces can be found in the Helm kube package](https://github.com/helm/helm/blob/main/pkg/kube/interface.go).

#### Helm CLI Interface

The Helm CLI is used by both people and automation. The CLI interface is something that can't break until there is a new major API release.

An example of this is with date format. There is one place where Helm uses a different date format from other places. And, we can't change the default to be consistent until Helm v4 is released. Tools parse this output and it would break the users of those tools to make a change there.

Specific details on this are documented in the [backwards compatibility HIP](https://github.com/helm/community/blob/main/hips/hip-0004.md).

#### Charts Interface

"Don't break my charts" -- anonymous

There are thousands and thousands of charts being publicly distributed between various people and systems. If the charts interface is broken it will break the experience for all of the people involved. Pitch forks will come out on a breaking change there. This is a no-go. Every change needs to be backwards compatible.

### Expert Target Users

Most Helm users are not experts in Helm or Kubernetes. We should not expect most users to be experts. If the UX of a change expects someone to be an expert, there needs to be changes.

### Tests, Tests, Tests

Changes should typically include a test. Tests are vital to make sure regressions aren't accidentally introduced later.

It's also important to look at any tests a pull request changed. A changed test may be because there was a regression or breaking change introduced.
