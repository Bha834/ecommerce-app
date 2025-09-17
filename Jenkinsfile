pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { 
        git branch: 'main',
          url: 'https://github.com/Bha834/ecommerce-app.git' }
    }
    stage('Build Docker') {
      steps { sh 'docker build -t myshop:v1 .' }
    }
    stage('Push DockerHub') {
      steps { sh 'docker push myshop:v1' }
    }
  }
}
