

pipeline {
    agent any

    environment {
        APP_IMAGE = 'my-devops-app:v3'
        DOCKERHUB_IMAGE = 'docker.io/mohammedzakariyya/my-devops-app:v3'
        DEPLOY_CONTAINER = 'project1-1-app'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Starting Project 1.1 CI/CD pipeline'
                echo 'Source code checkout completed'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application image v3'
                sh 'podman build -t my-devops-app:v3 .'
            }
        }

        stage('Validate') {
            steps {
                echo 'Validating image'
                sh 'podman image exists my-devops-app:v3'
                sh 'podman images'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the new image in a temporary container'
                sh '''
                    set -e

                    podman rm -f project1-1-test 2>/dev/null || true

                    podman run -d \
                        --name project1-1-test \
                        -p 18083:80 \
                        my-devops-app:v3

                    cleanup() {
                        podman rm -f project1-1-test >/dev/null 2>&1 || true
                    }
                    trap cleanup EXIT

                    for i in $(seq 1 15); do
                        if curl -fsS http://localhost:18083/; then
                            echo "Temporary application test passed"
                            exit 0
                        fi
                        sleep 2
                    done

                    echo "Temporary application test failed"
                    exit 1
                '''
            }
        }

        stage('Publish to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_TOKEN'
                    )
                ]) {
                    sh '''
                        set -e
                        set +x

                        printf '%s' "$DOCKERHUB_TOKEN" | podman login docker.io \
                            --username "$DOCKERHUB_USER" \
                            --password-stdin

                        podman tag my-devops-app:v3 "$DOCKERHUB_IMAGE"
                        podman push "$DOCKERHUB_IMAGE"

                        podman logout docker.io
                    '''
                }
            }
        }

        stage('Automatic Deployment') {
            steps {
                echo 'Deploying application on port 8083'
                sh '''
                    set -e

                    podman rm -f "$DEPLOY_CONTAINER" 2>/dev/null || true

                    podman run -d \
                        --name "$DEPLOY_CONTAINER" \
                        --restart unless-stopped \
                        -p 8083:80 \
                        "$APP_IMAGE"
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo 'Checking the deployed application'
                sh '''
                    set -e

                    for i in $(seq 1 15); do
                        if curl -fsS http://localhost:8083/; then
                            echo "Deployment health check passed"
                            exit 0
                        fi
                        sleep 2
                    done

                    echo "Deployment health check failed"
                    exit 1
                '''
            }
        }
    }

    post {
        success {
            echo 'Project 1.1 automatic deployment completed successfully'
        }

        failure {
            echo 'Project 1.1 pipeline failed. Check the console output'
        }
    }
}
