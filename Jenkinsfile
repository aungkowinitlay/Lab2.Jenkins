pipeline {
    agent none
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_CREDENTIALS')
        BACKEND_IMAGE = "aungkowin/flask-backend:latest"
        FRONTEND_IMAGE = "aungkowin/nginx-frontend:latest"
    }
    stages {
        stage('Checkout') {
            agent { label 'VM1' }
            steps {
                git branch: 'main', url: 'https://github.com/aungkowinitlay/Lab2.Jenkins.git'
            }
        }
        stage('Test VM2 Connection') {
            agent { label 'VM2' }
            steps {
                sh 'echo "Successfully connected to VM2"'
            }
        }
        stage('Build Backend Image') {
            agent { label 'VM1' }
            steps {
                sh """
                    docker build -t ${BACKEND_IMAGE} -f backend/Dockerfile.backend ./backend
                """
            }
        }
        stage('Build Frontend Image') {
            agent { label 'VM1' }
            steps {
                sh """
                    docker build -t ${FRONTEND_IMAGE} -f frontend/Dockerfile.frontend ./frontend
                """
            }
        }
        stage('Test Backend') {
            agent { label 'VM1' }
            steps {
                sh """
                    echo "Running backend tests..."
                    # Example: docker run ${BACKEND_IMAGE} pytest
                """
            }
        }
        stage('Login to Docker Hub') {
            agent { label 'VM1' }
            steps {
                sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                """
            }
        }
        stage('Push Images') {
            agent { label 'VM1' }
            steps {
                sh """
                    docker push ${BACKEND_IMAGE}
                    docker push ${FRONTEND_IMAGE}
                """
            }
        }
        stage('Deploy Backend') {
            agent { label 'VM3' }
            steps {
                sh """
                    docker-compose -f docker-compose.yml down || true
                    docker-compose -f docker-compose.yml up -d flask-backend
                """
            }
        }
        stage('Deploy Frontend') {
            agent { label 'VM2' }
            steps {
                sh """
                    docker-compose -f docker-compose.yml down || true
                    docker-compose -f docker-compose.yml up -d nginx-frontend
                """
            }
        }
    }
    post {
        always {
            agent { label 'VM1' }
            steps {
                sh """
                    docker logout
                """
            }
        }
        success {
            agent { label 'VM1' }
            steps {
                echo "Pipeline completed successfully!"
            }
        }
        failure {
            agent { label 'VM1' }
            steps {
                echo "Pipeline failed!"
            }
        }
    }
}