# Project Organization

Helm is more than just a single codebase. There isn't just one group that includes all the "Helm maintainers". Understanding what the Helm project provides and how it's organized is important to navigate and communicate what Helm can do.

## The Sub-Projects

Helm as an umbrella project has multiple sub-projects. The primary project is [Helm](https://github.com/helm/helm). This is followed by a series of sub-projects that include:

### Charts

This originally started out as the group that maintained the charts repository. This was a Helm repository backed by a Git repository on GitHub. It started small as an examples repo and later expanded to be a central charts repo. Such a large repository became unmaintainable so it was deprecated with multiple distributed repositories instead. You can search these using [Artifact Hub](https://artifacthub.io/).

The charts team had developed tools to manage the large charts repo. These tools were turned into stand-alone tools that others, who maintain their distributed Helm repositories, can use. This includes tools like [chart-testing](https://github.com/helm/chart-testing) and various GitHub actions.

### Chartmuseum

A Helm repository is an `index.yaml` file along with a collection of archives. The Helm client isn't the only tool that can generate it. For those who want a dynamic tool there is [chartmuseum](https://github.com/helm/chartmuseum). This is another sub-project of Helm.

### Web Team

A web presence and documentation is essential for a project like Helm. This is where the [website](https://github.com/helm/helm-www) and its maintainers come in.

## Organization Maintainers

To look over all the various projects we have the Org Maintainers. This group is responsible for the Helm project as a whole while not being technically responsible for any of the code bases.

The details of all of this can be found in the [governance](https://github.com/helm/community/tree/main/governance).
