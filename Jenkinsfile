pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './gradlew test'
            }
        }
        stage('Scan') {
            steps{
                sh 'docker info'
            }
        }
    }
}