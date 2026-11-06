pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Starting CI/CD pipeline'
                echo 'Source code checkout stage'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application'
                sh 'podman build -t my-devops-app:v2 .'
            }
        }

        stage('Validate') {
            steps {
                echo 'Validating container image'
                sh 'podman images'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application availability'
                sh 'curl -f http://localhost:8082'
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
                        set +x

                        echo "Logging in to Docker Hub"

                        printf '%s' "$DOCKERHUB_TOKEN" | podman login docker.io \
                            --username "$DOCKERHUB_USER" \
                            --password-stdin

                        echo "Tagging application image"

                        podman tag my-devops-app:v2 \
                            docker.io/mohammed-devops-engineer/my-devops-app:v2

                        echo "Pushing image to Docker Hub"

                        podman push docker.io/mohammed-devops-engineer/my-devops-app:v2

                        podman logout docker.io
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully'
        }

        failure {
            echo 'CI/CD pipeline failed. Check the console output'
        }
    }
}
