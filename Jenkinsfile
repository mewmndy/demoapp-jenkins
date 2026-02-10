pipeline {
  agent any

  parameters {
    choice(name: 'DEPLOY_TYPE', choices: ['docker'], description: 'เลือกวิธี deploy')
  }

  environment {
    IMAGE_NAME = "demoapp"
    CONTAINER_NAME = "demoapp_container"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'git@github.com:mewmndy/demoapp-jenkins.git'
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          docker stop $CONTAINER_NAME || true
          docker rm $CONTAINER_NAME || true
          docker run -d \
            --name $CONTAINER_NAME \
            -p 3000:3000 \
            $IMAGE_NAME:latest
        '''
      }
    }
  }
}
