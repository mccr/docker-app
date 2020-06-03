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
            steps{
                aquaMicroscanner imageName: 'mcalder/docker-scan-demo:firsttry', notCompliesCmd: 'exit 4', onDisallowed: 'fail', outputFormat: 'html'
            }
        }
    }
}