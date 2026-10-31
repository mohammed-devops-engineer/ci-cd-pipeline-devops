# ci-cd-pipeline-devops
End-to-End CI/CD Pipeline using Jenkins, Docker, Kubernetes

## Current Project Status

- Created a static HTML web application.
- Built the container image using Podman and Nginx Alpine.
- Started the application container.
- Published host port 8081 to container port 80.
- Verified the application using curl.

## Run Locally

Build the image:

    podman build -t my-devops-app:v1 .

Start the container:

    podman run -d --name my-devops-container -p 8081:80 my-devops-app:v1

Test the application:

    curl http://localhost:8081
