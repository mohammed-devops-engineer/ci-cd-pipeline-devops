# CI/CD Pipeline Automation Using Jenkins and Podman

## 1. Project Overview

This project demonstrates an automated Continuous Integration and Continuous Delivery (CI/CD) workflow for a simple static web application.

The application is built using Nginx Alpine and packaged into a container image using Podman. Jenkins automates the pipeline by retrieving source code from GitHub, building the container image, and validating the build.

The purpose of this project is to demonstrate practical DevOps skills through a hands-on lab environment.

## 2. Project Objectives

- Maintain application source code in GitHub.
- Automate pipeline execution using Jenkins.
- Build container images using Podman.
- Validate successful image creation.
- Document the CI/CD workflow and troubleshooting procedures.
- Extend the pipeline towards container registry publishing and Kubernetes deployment.

## 3. Technology Stack

| Technology | Purpose |
|---|---|
| Rocky Linux 9 | Operating system for the lab |
| Git | Version control |
| GitHub | Source code repository |
| Jenkins | CI/CD automation |
| Podman | Container image building and runtime |
| Nginx Alpine | Web server and base container image |
| HTML | Static web application |
| Kubernetes | Planned future deployment platform |
| Prometheus and Grafana | Planned monitoring integration |

## 4. Project Architecture

The current workflow is:

1. Source code is maintained in GitHub.
2. Jenkins retrieves the project repository.
3. Jenkins executes the declarative pipeline.
4. Podman builds the container image using the Dockerfile.
5. Jenkins validates the generated image.

```text
Developer
    |
    v
GitHub Repository
    |
    v
Jenkins Pipeline
    |
    v
Podman Image Build
    |
    v
Image Validation
```

## 5. Repository Structure

```text
ci-cd-pipeline-devops/
├── app/
│   └── index.html
├── Dockerfile
├── Jenkinsfile
└── README.md
```

## 6. Jenkins Pipeline Stages

The Jenkins pipeline currently contains the following stages:

| Stage | Description |
|---|---|
| Checkout | Retrieves source code from GitHub |
| Build | Builds the container image using Podman |
| Validate | Lists and verifies the generated image |

The pipeline also includes post-build success and failure messages.

## 7. Run the Application Locally

### Build the container image

```bash
podman build -t my-devops-app:v1 .
```

### Start the application container

```bash
podman run -d --name my-devops-container -p 8081:80 my-devops-app:v1
```

### Verify the application

```bash
curl http://localhost:8081
```

The application should return the HTML content served by Nginx.

Note: The commands above describe the local application setup. The Jenkins pipeline currently builds a separate image tagged `my-devops-app:v2` under the Jenkins account's rootless Podman storage.

## 8. Current Implementation Status

- [x] Static HTML application created.
- [x] Dockerfile configured with Nginx Alpine.
- [x] GitHub repository created.
- [x] Jenkins installed and configured.
- [x] Jenkins connected to the GitHub feature branch.
- [x] Rootless Podman configured for Jenkins.
- [x] Jenkins pipeline successfully executed.
- [x] Container image built and validated by Jenkins.
- [ ] Automated image publishing to Docker Hub.
- [ ] Kubernetes deployment.
- [ ] Prometheus and Grafana monitoring integration.

## 9. Future Enhancements

- Automate container image publishing to Docker Hub.
- Deploy the application to Kubernetes.
- Configure application health checks.
- Integrate Prometheus and Grafana for monitoring.
- Improve pipeline notifications and error handling.

## 10. Project Repository

GitHub: https://github.com/mohammed-devops-engineer/ci-cd-pipeline-devops

---

**Project Type:** Hands-on DevOps Portfolio Project  
**Pipeline Status:** Jenkins build and image validation successful  
**Container Engine:** Podman
