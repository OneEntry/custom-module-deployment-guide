# OneEntry HeadlessCMS client module

# General Information

This repository contains an example of GitLab CI/CD and GitHub Actions for deploying custom modules to the **production** environment of the CMS. Currently, the following languages are supported:

-   node
-   php
-   python

For each of the languages listed above, there are specific development requirements. To get started, the following files need to be created in your repository:

-   CI

| System | Source Path              | Destination Path |
| ------ | ------------------------ | ---------------- |
| GitLab | ci/gitlab/.gitlab-ci.yml | ./.gitlab-ci.yml |
| GitHub | ci/github/\*             | ./\*             |

-   Dockerfile

| Language | Source Path      | Destination Path |
| -------- | ---------------- | ---------------- |
| node     | docker/node/\*   | ./.docker/\*     |
| php      | docker/php/\*    | ./.docker/\*     |
| python   | docker/python/\* | ./.docker/\*     |

# CI/CD

In the simplest case, the CI will consist of two stages: building a Docker image and deploying it to the production environment. The .gitlab-ci.yml and .github/workflows/\* files in this repository serve as a base that the client can extend with additional stages if necessary (e.g., adding automated tests or lint checks). All stages will be executed on the client's resources, meaning the client must set up GitLab/GitHub runners or use shared runners.

For shared GitLab runners, any of the following tags should be used:

-   GitLab (more details can be found here - https://docs.gitlab.com/ee/ci/runners/hosted_runners/linux.html):
    ** saas-linux-medium-amd64 (preferred)
    ** gitlab-org-docker

The pipeline is triggered by setting a tag in the format prod-v\*!

**IMPORTANT!** Before use, all variables prefixed with $CHANGE_ME in the file must be replaced with actual values for your project!

## build

By default, the Docker image with the application is expected to be stored in the client's GitLab/GitHub container registry. To download and deploy this image in the production environment, the client must:

-   For GitLab - create a project token with **read_registry** permissions, which must then be passed at the deploy stage. More details can be found here - https://docs.gitlab.com/ee/user/project/settings/project_access_tokens.html#scopes-for-a-project-access-token.
-   For GitHub - create a personal token with **read:packages** permissions, which must then be passed at the deploy stage. More details can be found here - https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-with-a-personal-access-token-classic.

Additionally, in the CI files, the runner tags for job execution must be adjusted:

-   For GitLab - replace ${CHANGE_ME} in the **tags** block with your runner tags
-   For GitHub - replace ${CHANGE_ME} in the **runs-on** field with your runner labels

**IMPORTANT!** This example assumes the use of Docker-in-Docker! If using a GitLab self-hosted runner, this functionality must be ensured. More information can be found at https://docs.gitlab.com/ee/ci/docker/using_docker_build.html#use-docker-in-docker

## deploy

Deployment is performed through the cms-admin web interface. The following details need to be provided:

-   the image of your application
-   authentication data for the Docker registry

# Development Requirements

## General

You need to:

-   configure the env DEVELOPER_API_HOST - this will contain the address of the host where the developer API will be accessible. The format is as follows - $scheme://$host:$port.
-   specify the tag of the Docker image with the required language version
-   specify the command to run your application

## node

You need to:

-   prepare a package.json and package-lock.json for installing necessary packages using the npm package manager

## php

You need to:

-   Manually add a list of missing packages to the image and install them in the Dockerfile

## python

You need to:

-   Prepare a requirements.txt file for installing packages using the pip package manager
