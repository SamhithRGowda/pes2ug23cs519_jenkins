pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh '''
                docker rm -f backend1 backend2 || true
                docker run -d --name backend1 --network lab-net backend-app
                docker run -d --name backend2 --network lab-net backend-app
                '''
            }
        }

        stage('Deploy Nginx Load Balancer') {
            steps {
                sh '''
                docker rm -f nginx-container || true
                docker build -t nginx-app ./nginx
                docker run -d --name nginx-container --network lab-net -p 8081:80 nginx-app
                '''
            }
        }
    }
}
