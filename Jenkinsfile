pipeline {
    agent any

    environment {
        // Use Jenkins credentials (create one in Jenkins → Manage Jenkins → Credentials)
        // with ID = dockerhub-cred (username + password)
        DOCKER_CREDENTIALS = credentials('dockerhub-cred')
        IMAGE_NAME = "myshop"
        IMAGE_TAG = "v1"
    }
    stage('Debug') {
    steps {
        sh 'echo "DockerHub username is $DOCKER_CREDENTIALS_USR"'
    }
}


    

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Bha834/ecommerce-app.git'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push DockerHub') {
            steps {
                sh '''
                    echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_CREDENTIALS_USR/$IMAGE_NAME:$IMAGE_TAG
                    docker push $DOCKER_CREDENTIALS_USR/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
