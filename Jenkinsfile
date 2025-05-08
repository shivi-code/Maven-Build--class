pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your/repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Run in Docker') {
            steps {
                script {
                    docker.image(IMAGE_NAME).inside {
                        sh 'echo "Run your computation here"'
                    }
                }
            }
        }

        stage('Deploy to Container') {
            steps {
                sh '''
                docker run -d --name myapp_container ${IMAGE_NAME}
                '''
            }
        }

        stage('Clean Up') {
            steps {
                sh '''
                docker stop myapp_container
                docker rm myapp_container
                docker rmi ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
