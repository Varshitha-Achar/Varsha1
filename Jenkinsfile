pipeline {
    agent any

    environment {
        // ID of the credentials stored in Jenkins
        DOCKERHUB_CREDENTIALS = 'varshz' 
        // Your Docker Hub image repository
        IMAGE_NAME = 'varshz/java-app'
    }

    stages {
        stage('Build Java Application') {
            steps {
                // Compiles the Java file
                bat 'javac HelloWorld.java'
            }
        }

        stage('Run Java Program') {
            steps {
                // Runs the program to ensure it works before bottling it up
                bat 'java HelloWorld'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Builds the image using the local Dockerfile
                bat "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {
                    
                    // Using PowerShell to avoid the 'trailing space' bug in Windows Batch
                    powershell 'echo $env:PASS | docker login -u $env:USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push ${IMAGE_NAME}:latest"
            }
        }
    }
    
    post {
        always {
            // Clean up the local image to save disk space on the Jenkins agent
            bat "docker rmi ${IMAGE_NAME}:latest || exit 0"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for credential or syntax errors.'
        }
    }
}
