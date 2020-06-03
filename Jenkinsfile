pipeline {
    agent any

    environment {
        registry = "mcalder/docker-app"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'gradle:jdk'
                }
            }
            steps {
                echo 'Testing..'
                sh './gradlew test'
            }
        }
        stage ("lint dockerfile") {
            agent {
                docker {
                    image 'hadolint/hadolint:latest-debian'
                }
            }
            steps {
                checkout scm
                sh 'hadolint Dockerfile | tee -a hadolint_lint.txt'
            }
            post {
                always {
                    archiveArtifacts 'hadolint_lint.txt'
                }
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
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Scan') {
            steps{
                aquaMicroscanner imageName: dockerImage, notCompliesCmd: 'exit 4', onDisallowed: 'fail', outputFormat: 'html'
            }
        }
    }
}