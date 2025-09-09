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
                    sh "docker ru -d -p 8081:8080 --name my-java-app ${imageName}:${imageTag}"
                }
            }
        }
    }

    // This section defines what happens after the pipeline finishes.
    post {
        // This block runs if the pipeline completes successfully.
        success {
            emailext (
                to: 'awsdevops95117@gmail.com',
                subject: "SUCCESS: Jenkins Pipeline Job '${env.JOB_NAME}'",
                body: "Pipeline '${env.JOB_NAME}' build #${env.BUILD_NUMBER} was successful.\n\n" +
                      "Check it out at: ${env.BUILD_URL}"
            )
        }
        // This block runs if the pipeline fails.
        failure {
            emailext (
                to: 'awsdevops95117@gmail.com',
                subject: "FAILURE: Jenkins Pipeline Job '${env.JOB_NAME}'",
                body: "Pipeline '${env.JOB_NAME}' build #${env.BUILD_NUMBER} has failed.\n\n" +
                      "Check the console output for details: ${env.BUILD_URL}/console"
            )
        }
    }
}
