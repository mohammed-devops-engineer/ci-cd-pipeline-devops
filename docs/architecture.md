# Project 1 — CI/CD Pipeline Architecture

## 1. Introduction

This document describes the architecture of the CI/CD pipeline project implemented using GitHub, Jenkins, Rocky Linux 9, and rootless Podman.

The project demonstrates how application source code is retrieved from a GitHub repository, processed by Jenkins, packaged into a container image, and validated.

## 2. Architecture Overview

The current CI/CD workflow consists of the following components:

1. Developer maintains application source code.
2. GitHub stores the source code and Jenkins pipeline definition.
3. Jenkins retrieves the repository from GitHub.
4. Jenkins executes the declarative pipeline.
5. Podman builds the container image using the Dockerfile.
6. Jenkins validates the generated image.

## 3. Architecture Diagram

```text
             Developer
                 |
                 v
          GitHub Repository
                 |
                 v
          Jenkins Pipeline
                 |
          +------+------+
          |             |
          v             v
       Checkout       Build
                        |
                        v
                  Rootless Podman
                        |
                        v
                  Container Image
                        |
                        v
                    Validate
```

## 4. Component Description

### 4.1 Rocky Linux 9

Rocky Linux 9 is the operating system used for the DevOps laboratory.

It hosts Jenkins, Podman, Git, and the application development environment.

### 4.2 Git and GitHub

Git is used for source code version control.

GitHub hosts the project repository:

https://github.com/mohammed-devops-engineer/ci-cd-pipeline-devops

The project uses the feature branch:

`feature/project-1-foundation`

### 4.3 Jenkins

Jenkins is the automation server responsible for executing the CI/CD pipeline.

The Jenkins job is named:

`ci-cd-pipeline`

The pipeline definition is maintained in the `Jenkinsfile` stored in the GitHub repository.

### 4.4 Podman

Podman is the container engine used in this laboratory.

Jenkins runs Podman in rootless mode under its dedicated Linux service account.

This provides container image building without granting Jenkins unrestricted root privileges.

### 4.5 Nginx Alpine

Nginx Alpine is used as the base image for the static web application.

The Dockerfile copies the application's HTML file into the Nginx web server document directory.

## 5. Jenkins Pipeline Stages

| Stage | Responsibility |
|---|---|
| Checkout | Retrieves source code from GitHub |
| Build | Builds the container image using Podman |
| Validate | Lists and validates the generated image |

The pipeline also provides post-build success and failure messages.

## 6. Container Image Details

| Parameter | Value |
|---|---|
| Image name | my-devops-app |
| Image tag | v2 |
| Container engine | Podman |
| Base image | nginx:alpine |
| Build location | Jenkins workspace |

The image is stored in Jenkins' rootless Podman storage and is separate from the image storage of the `mohammed` Linux account.

## 7. Security Considerations

- Jenkins runs under a dedicated Linux service account.
- Jenkins uses rootless Podman.
- Jenkins does not require unrestricted root privileges to build the image.
- GitHub authentication uses a Personal Access Token when required.
- Access tokens must never be committed to the source code repository.

## 8. Future Architecture Enhancements

The following components are planned but are not yet implemented in this project:

- Docker Hub integration for publishing container images.
- Kubernetes integration for application deployment.
- Prometheus for monitoring.
- Grafana for monitoring dashboards.
- Automated application health checks.

## 9. Current Architecture Status

The GitHub-to-Jenkins-to-Podman image build and validation workflow has been successfully executed.

**Current status:** CI/CD pipeline build and image validation successful.

**Operating system:** Rocky Linux 9.

**Container engine:** Rootless Podman.
