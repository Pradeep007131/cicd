pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-node-app:${BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Pradeep007131/cicdjobs.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).inside {
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh """
                    docker pull my-node-app:${BUILD_ID}
                    docker tag my-node-app:${BUILD_ID} my-node-app:latest
                    cd /path/to/docker-compose-directory
                    docker-compose down
                    docker-compose up -d
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

