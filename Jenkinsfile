pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t nextjs-test:latest .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker stop nextjs-test || true
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker rm nextjs-test || true
                '''
            }
        }

        stage('Deploy New Container') {
            steps {
                sh '''
                    docker run -d \
                      --name nextjs-test \
                      -p 3000:3000 \
                      nextjs-test:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    docker ps
                    docker logs --tail 20 nextjs-test
                '''
            }
        }
    }
}
