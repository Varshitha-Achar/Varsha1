stage('Push Image to Repository') {
    steps {
        script {
            // 1. securely injects your registry credentials
            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', 
                                              passwordVariable: 'DOCKER_PASSWORD', 
                                              usernameVariable: 'DOCKER_USERNAME')]) {
                
                // 2. Log in securely (piping the password avoids exposing it in logs)
                sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                
                // 3. Push the specific version tag
                sh "docker push yourusername/your-image-name:${env.BUILD_NUMBER}"
                
                // 4. (Optional) Push the 'latest' tag
                sh "docker push yourusername/your-image-name:latest"
            }
        }
    }
}
