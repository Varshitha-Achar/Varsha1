pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = 'varshz'
        IMAGE_NAME = 'varshz'
}

stages {

stage(&#39;Build Java Application&#39;) {
steps {
bat &#39;javac HelloWorld.java&#39;
}
}

stage(&#39;Run Java Program&#39;) {
steps {
bat &#39;java HelloWorld&#39;
}
}

stage(&#39;Build Docker Image&#39;) {
steps {
bat &#39;docker build -t %IMAGE_NAME%:latest .&#39;
}
}

stage(&#39;Login to DockerHub&#39;) {
steps {
withCredentials([usernamePassword(

credentialsId: 'Docker-credentials'
usernameVariable: &#39;USER&#39;,
passwordVariable: &#39;PASS&#39;)]) {

bat &#39;echo %PASS%| docker login -u %USER% --password-stdin&#39;
}
}
}

stage(&#39;Push Docker Image&#39;) {
steps {
bat &#39;docker push %IMAGE_NAME%:latest&#39;
}
}
}
}
