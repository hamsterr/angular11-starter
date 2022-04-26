node {
  stage('Clean Workspace'){
    cleanWs()
  }

  stage("Main build") {
    docker.image('node:latest').pull()
    docker.image('ismail0352/chrome-node').pull()

    stage('Checkout SCM') {
      checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/hamsterr/angular11-starter.git']]])
    }
    
    // Permorming Install and Lint
    docker.image('node:latest').inside {
      stage('Install') {
        sh label:
          'Running npm install',
        script: '''
          ls
          npm install
        '''
      }
    }

    stage('Get test dependency') {
      sh label:
        'Downloading chrome.json',
      script: '''
        wget https://raw.githubusercontent.com/jfrazelle/dotfiles/master/etc/docker/seccomp/chrome.json -O $WORKSPACE/chrome.json
      '''
    }

    stage ('Build') {
      docker.image('node:latest').inside {
        sh label:
          'Running npm run build',
        script: '''
          node --version
          npm run build
        '''
      }
    }
  }

  stage("Deploy") {
    def containerName = "hello-nginx"
    // Any change in Volume will automatically result in Hot Reload of Nginx
    def rc = sh (script: "docker inspect -f '{{.State.Running}}' ${containerName}", returnStatus: true)
    if(rc == 0) {
      echo "Container ${containerName} exists..."
      try {
        echo "Nginx will reload changes from the mounted file system..."
        timeout(time: 120, unit: 'SECONDS') { // change to a convenient timeout for you
          input(
            message: 'Click "Create" to Discard old container and create a new one?\nClick "Abort" to keep the old one\nIf nothing is clicked "Abort" will be triggered', ok: 'Create')
          }
          echo "Removing old container and creating a new one..."
          sh "docker rm -f hello-nginx"
          sh "docker run -d -p 80:80 -v $WORKSPACE/hello-world-node/dist/hello-world-node:/usr/share/nginx/html/ --name ${containerName} nginx"

      }
      catch(err) { // timeout reached or input false
        echo "Doing Nothing!"
      }
    }
    else
    {
      echo "Container ${containerName} does not exist... Creating..."
      sh "docker run -d -p 80:80 -v $WORKSPACE/hello-world-node/dist/hello-world-node:/usr/share/nginx/html/ --name ${containerName} nginx"
    }
  }
}