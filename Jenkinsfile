pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Mrunmay1312/flask-ci-cd.git'
        DOCKER_IMAGE = 'mrunmay1312/flask-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git REPO_URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    sshagent(['deploy-key']) {
                        sh '''
                            docker pull ${DOCKER_IMAGE}:latest && docker run -d -p 5000:5000 ${DOCKER_IMAGE}:latest
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}