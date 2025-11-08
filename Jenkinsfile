pipeline {
    agent any
    stages {
        stage ('pull code from github') {
            steps {
                // clone repository from github
                git branch: 'main' , url: 'https://github.com/chandan0830/test1.git'
            }
        }
        stage ('build docker image') {
            steps {
                sh  'docker build -t web-v1 .'
                }
        }

        stage ('push image to docker hub') {
            steps {
                sh '''
                echo " login to docker hub "
                docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD

                echo " image tag"
                docker tag web-v1:latest $DOCKERHUB_USERNAME/web-v1:latest

                echo " push image"
                docker push $DOCKERHUB_USERNAME/web-v1:latest

                echo "docker logout"
                docker logout 
                '''
            }
        }

        stage ('deploy to kubernetes') {
            steps {
                sh '''
                echo " deploy application on kubernetes"
                kubectl apply -f deployment.yaml
                '''
            }
        }

    }
}
