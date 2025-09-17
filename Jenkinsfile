pipeline {
    agent any

    environment {
        // Jenkins credentials ka ID (dockerhub-cred)
        DOCKER_CREDENTIALS = credentials('dockerhub-cred')
        IMAGE_NAME = "myshop"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Debug') {
            steps {
                sh 'echo "DockerHub username is $DOCKER_CREDENTIALS_USR"'
            }
        }

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

        stage('Deploy') {
            steps {
                // Agar container pehle se run ho to usko delete karke naya run karega
                sh '''
                    docker rm -f myshop-container || true
                    docker run -d -p 5000:5000 --name myshop-container $DOCKER_CREDENTIALS_USR/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
