pipeline {
    agent any

    stages {
//        stage('Test') {
//            steps {
//                echo 'Testing..'
//                sh './gradlew test'
//            }
//        }
//        dir('repo') {
//            checkout scm
//        }
        stage('Scan') {
            steps {
                checkout scm
                sh 'docker build --build-arg=token=YjY1NzJlYzdhZmEz --no-cache .'
            }
        }
    }
}