pipeline {
    agent any

    stages {
        // Stage 1: Build the Java application using a Docker agent.
        stage('Build') {
            steps {
                docker.image('maven:3.8.6-openjdk-8').inside {
                    sh 'mvn clean package'
                }
            }
        }

        // Stage 2: Docker Build and Deploy
        stage('Docker Build & Deploy') {
            steps {
                script {
                    // Define the Docker image name and tag.
                    def imageName = "my-java-app"
                    def imageTag = "${env.BUILD_NUMBER}"

                    // Build the Docker image from the Dockerfile.
                    sh "docker build -t ${imageName}:${imageTag} ."

                    // Stop and remove any previously running containers
                    sh 'docker stop my-java-app || true'
                    sh 'docker rm my-java-app || true'

                    // Run the new container, mapping the ports
                    sh "docker run -d -p 8080:8080 --name my-java-app ${imageName}:${imageTag}"
                }
            }
        }
    }
}
