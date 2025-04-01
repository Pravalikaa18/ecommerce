pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-ecommerce-backend"
        DOCKER_TAG = "${BUILD_NUMBER}"
        REGISTRY = "pravalikaa18/my-ecommerce"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Pravalikaa18/ecommerce-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    cd backend
                    docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                    """
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: '98897e03-c137-4438-8da4-dd1f33b63fba', url: '']) {
                        sh """
                        docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${REGISTRY}:${DOCKER_TAG}
                        docker push ${REGISTRY}:${DOCKER_TAG}
                        """
                    }
                }
            }
        }

        stage('Deploy to Local Docker') {
            steps {
                script {
                    sh """
                    docker stop ecommerce || true
                    docker rm ecommerce || true
                    docker run -d -p 5000:5000 --name ecommerce ${REGISTRY}:${DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
