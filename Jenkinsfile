pipeline {
  agent {
    docker {
      image 'docker:20.10.7-git'            // มี git ติดมาแล้ว
      args '-v /var/run/docker.sock:/var/run/docker.sock'
      reuseNode true
    }
  }

  options { skipDefaultCheckout(true) }     // เราจะ checkout เองใน stage

  environment {
    IMAGE = 'jenkins-demo-app:latest'
    CONTAINER_NAME = 'demo-app'
  }

  stages {
    stage('Checkout') {
      steps {
        // ใช้ URL จริงของ public repo (ไม่มี <username>)
        git branch: 'main', url: 'https://github.com/alifriduwan/jenkins-demo-app.git'
      }
    }

    stage('Build Image') {
      steps {
        sh '''
          docker version
          docker build -t "$IMAGE" .
        '''
      }
    }

    stage('Run Container') {
      steps {
        sh '''
          docker rm -f "$CONTAINER_NAME" >/dev/null 2>&1 || true
          docker run -d -p 5000:5000 --name "$CONTAINER_NAME" "$IMAGE"
        '''
      }
    }
  }

}
