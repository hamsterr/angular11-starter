pipeline {
      agent {
        docker {
          image 'node:latest' 
            args '-p 3000:3000  -v /var/jenkins_home/app:/app' 
        }
      }
      stages {
        stage('Build') { 
          steps {
            sh 'npm install' 
            sh 'ng  build --prod'
            sh 'cp ./dist /app'
          }
        }
      }
    }
