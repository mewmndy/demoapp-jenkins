pipeline {
  agent any

  parameters {
    string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'เวอร์ชันที่ต้องการ build')
  }

  environment {
    IMAGE_NAME = "demoapp"
    CONTAINER_NAME = "demoapp_container"
  }

  stages {

    stage('Build') {
      steps {
        sh '''
          echo "Building version ${APP_VERSION}"
          docker build -t $IMAGE_NAME:${APP_VERSION} .
        '''
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
            -e APP_VERSION=${APP_VERSION} \
            $IMAGE_NAME:${APP_VERSION}
        '''
      }
    }
  }
}
