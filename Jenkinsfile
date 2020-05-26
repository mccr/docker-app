pipeline {
    agent any

    stages {
        stage('Test') {
            agent { label 'gradle' }
            steps {
                echo 'Testing..'
                sh './gradlew test'
            }
        }
        stage('Scan') {
            steps{
                aquaMicroscanner imageName: 'openjdk', notCompliesCmd: 'exit 4', onDisallowed: 'fail', outputFormat: 'html'
            }
        }
    }
}