pipeline {
    agent any
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    app = docker.build("selcuks/sample-client:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            app.push("latest")
                            app.push("${env.BUILD_ID}")
                    }
                }
            }
        } 
        stage('Deploy') {
            steps {
                sh "kubectl apply -f sample-client-deployment.yaml"
            }
        }
    }
}