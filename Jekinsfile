pipeline {
    environment {
        dockerimagename = 'alyssajin/nodeapp'
        dockerImage = ''
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/Alyssajin/node-app-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image to Docker Hub') {
            environment {
                registryCredential = credentials('dockerhublogin')
            }

            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential ) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: 'deploymentservice.yaml', kubeconfigId: 'kubernetes')
                }
            }
        }
    }
}
