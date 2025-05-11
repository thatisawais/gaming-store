pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ecommerce-pipeline-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/thatisawais/gaming-store'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose -p ecommerce_pipeline -f docker-compose.yml up -d --build'
            }
        }

        stage('Post-Build Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
    }
}
