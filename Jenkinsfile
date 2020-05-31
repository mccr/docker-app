pipeline {
    agent any
    environment {
        SCANNER_TOKEN = credentials('scanner-token')
    }
    stages {
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './gradlew test'
            }
        }
        stage('Scan') {
            steps {
                checkout scm
                sh './gradlew build'
                sh 'docker build --build-arg=token=$SCANNER_TOKEN --no-cache .'
            }
        }
    }
}