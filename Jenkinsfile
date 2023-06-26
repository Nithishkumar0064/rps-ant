#!groovy
pipeline {
    environment {
        registry = "nithishnithi/rps-ant"
        registryCredential = 'docker-credential'
    }
    agent any
    stages {
        stage('Clone the Git Repository') {
            steps {
                git credentialsId: 'git-credential', url: 'https://github.com/Nithishkumar0064/rps-ant.git'
            }
        }
        stage('Building docker image') {
            steps {
                sh 'echo Building docker image'
                script {
                    buildDate = new Date()
                    image = docker.build("${registry}:$BUILD_NUMBER")
                }
            }
        }
        stage('Registry-Push') {
            steps {
                sh 'echo Registry-push'
                script {
                    docker.withRegistry('', registryCredential) {
                        image.push()
                        image.push('latest')
                    }
                }
            }
        }
        stage('CleanUp workspace') {
            steps {
                sh ' echo CleanUp'
                sh "docker rmi ${registry}:${BUILD_NUMBER}"
            }
        }
    }
}
