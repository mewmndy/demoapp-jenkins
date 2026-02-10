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
    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          docker stop $CONTAINER_NAME || true
          docker rm $CONTAINER_NAME || true
          docker run -d -p 3000:3000 --name $CONTAINER_NAME $IMAGE_NAME:latest
        '''
      }
    }
  }
}
