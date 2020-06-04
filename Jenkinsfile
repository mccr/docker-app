pipeline {
    agent any

    environment {
        registry = "mcalder/docker-app"
        registryCredential = 'dockerhub'
        dockerImage = ''
        baseImage = 'openjdk:11'
        imageName = "mcalder/docker-app:$BUILD_NUMBER"
    }

    stages {
//        stage('Test') {
//            agent {
//                docker {
//                    image 'gradle:jdk'
//                }
//            }
//            steps {
//                echo 'Testing..'
//                sh './gradlew test'
//            }
//        }
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
                sh 'docker trust inspect $baseImage >> signatures.txt'
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
                sh 'docker trust sign $imageName'
            }
        }
        stage('Scan') {
            steps{
                aquaMicroscanner imageName: imageName, notCompliesCmd: 'exit 4', onDisallowed: 'fail', outputFormat: 'html'
            }
        }
    }
}