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
