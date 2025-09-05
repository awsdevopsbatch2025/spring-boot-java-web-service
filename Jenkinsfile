pipeline {
    agent any

    stages {
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }


        stage('Docker Build & Deploy') {
            steps {
                script {
                    def imageName = "my-java-app"
                    def imageTag = "${env.BUILD_NUMBER}"

                    sh "docker build -t ${imageName}:${imageTag} ."

                    sh 'docker stop my-java-app || true'
                    sh 'docker rm my-java-app || true'

                    sh "docker run -d -p 8081:8080 --name my-java-app ${imageName}:${imageTag}"
                }
            }
        }
    }
}
