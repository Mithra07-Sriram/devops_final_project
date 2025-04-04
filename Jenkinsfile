pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "mithras72/html-deploy"
        DOCKER_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "docker"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Mithra07-Sriram/devops_final_project.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                echo "Logging into Docker Hub..."
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }
        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker container..."
                sh '''
                docker stop html-container || true
                docker rm html-container || true
                docker run -d -p 8081:80 --name html-container $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }
    post {
        success {
            echo "Deployment Successful! Access at http://your-server-ip:8081"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}

