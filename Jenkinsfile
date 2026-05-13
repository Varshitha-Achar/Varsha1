pipeline {
    agent any

    environment {
        // Defined here once to avoid typing errors later
        DOCKER_CREDS_ID = 'Docker-credentials'
        IMAGE_NAME      = 'shilpakevala/new_docker_image'
    }

    stages {
        stage('Build Java Application') {
            steps {
                // Compiles the Java source file
                bat 'javac HelloWorld.java'
            }
        }

        stage('Run Java Program') {
            steps {
                // Verifies the program runs before building the container
                bat 'java HelloWorld'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Builds the image. Using double quotes allows Groovy to inject the variable.
                bat "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CREDS_ID}",
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {
                    
                    // 1. We use PowerShell to handle the password pipe safely.
                    // 2. We use 'Write-Output' which handles special characters better than 'echo'.
                    powershell 'Write-Output $env:PASS | docker login -u $env:USER --password-stdin'
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
        success {
            echo "Successfully pushed ${IMAGE_NAME} to DockerHub."
        }
        cleanup {
            // Optional: Remove local image to save disk space on the Jenkins agent
            bat "docker rmi ${IMAGE_NAME}:latest || exit 0"
        }
    }
}
