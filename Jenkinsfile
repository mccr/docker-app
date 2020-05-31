pipeline {
    agent any

    stages {
//        stage('Test') {
//            steps {
//                echo 'Testing..'
//                sh './gradlew test'
//            }
//        }
        stage('Clone sources') {
            git url: 'https://github.com/mccr/docker-app.git'
        }
        stage('Scan') {
            steps{
                sh 'docker build --build-arg=token=YjY1NzJlYzdhZmEz --no-cache .'
            }
        }
    }
}