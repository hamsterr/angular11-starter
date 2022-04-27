pipeline {
    agent {
        docker { image 'node:16.14.2-alpine' }
    }
    stages {
        stage('SCM') {
            steps {
                sh 'node --version'
            }
        stage('Install') {
            steps {
                sh 'npm install'
            }
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}