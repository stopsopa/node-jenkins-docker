pipeline {
    environment {
        registry = "twlphaseii/simple-node-app"
        registryCredential = 'dockerhub'
    }

    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:8.12.0-alpine'
                }
            }
            steps {
                sh 'npm install' 
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

    }
}
