pipeline {
  agent any

  parameters {
    gitParameter(
      name: 'BRANCH_NAME',
      type: 'PT_BRANCH',
      defaultValue: 'main',
      branchFilter: '.*',
      description: 'เลือก branch จาก GitHub'
    )
  }

  environment {
    IMAGE_NAME = "demoapp"
    CONTAINER_NAME = "demoapp_container"
    REPO_URL = "https://github.com/mewmndy/demoapp-jenkins.git"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: params.BRANCH_NAME, url: env.REPO_URL
      }
    }

    stage('Build') {
      steps {
        sh '''
          echo "Building from branch: ${BRANCH_NAME}"
          docker build -t $IMAGE_NAME:${BRANCH_NAME} .
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
            -e APP_BRANCH=${BRANCH_NAME} \
            $IMAGE_NAME:${BRANCH_NAME}
        '''
      }
    }
  }
}
