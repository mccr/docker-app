pipeline {
    agent any

    environment {
        registry = "mcalder/docker-app"
        registryCredential = 'dockerhub'
        dockerImage = ''
        baseImage = 'openjdk:11'
        imageName = "mcalder/docker-app:$BUILD_NUMBER"
        SCANNER_TOKEN = credentials('scanner-token')
    }

    stages {
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
        stage ("verify signatures") {
            steps {
                sh 'docker trust inspect $baseImage | tee -a signatures.txt'
            }
            post {
                always {
                    archiveArtifacts 'signatures.txt'
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
            agent {
                docker {
                    image 'gradle:jdk'
                }
            }
            steps {
                checkout scm
                sh 'gradle build'
                sh 'docker build --build-arg=token=$SCANNER_TOKEN --no-cache .'
            }
        }
    }
}