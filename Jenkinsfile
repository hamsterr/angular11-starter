pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
          docker { image 'node:latest' }
      }
      steps { sh 'npm install' 
              sh 'npm run-script build'
      }
    }

    stage('Test') {
      parallel {
        stage('Static code analysis') {
            steps { sh 'npm run-script lint' }
        }
        stage('Unit tests') {
            steps { sh 'npm run-script test' }
        }
      }
    }
  }
}