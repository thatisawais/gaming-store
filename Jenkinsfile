pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                // Clone from main branch explicitly
                git branch: 'main', url: 'https://github.com/thatisawais/gaming-store.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose -p ecommerce_pipeline -f docker-compose.yml up -d --build'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
