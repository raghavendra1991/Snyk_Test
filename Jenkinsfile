// Declarative pipeline
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "raghaduvva/flaskapp"
        DOCKERHUB_CREDENTIALS = credentials('docker-hub')
        CONTAINER_NAME = "flaskapp"
        http_proxy = 'http://127.0.0.1:3128/'
        https_proxy = 'http://127.0.0.1:3128/'
        ftp_proxy = 'http://127.0.0.1:3128/'
        socks_proxy = 'socks://127.0.0.1:3128/'
    }

    stages {

        /* We do not need a stage for checkout here since it is done by default when using "Pipeline script from SCM" option. */
        
        stage ('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t $DOCKER_HUB_REPO:$BUILD_NUMBER .'       
            }
        }
        stage('Test Containers') {
            steps {
                echo 'Creating Conatiner Tesing Purpose'
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
                sh 'docker run -td --name $CONTAINER_NAME -p 5000:5000 --restart unless-stopped $DOCKER_HUB_REPO:$BUILD_NUMBER'
            }
        }

        stage ('Push Image ') {
            steps {
                echo 'Pushing Image'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR  --password-stdin && docker push $DOCKER_HUB_REPO:$BUILD_NUMBER'
            }
        }
        stage ('Testing Container ') {
            steps {
                echo 'Testing Container'
                sh 'curl localhost:5000'
            }
        }
    }
}
