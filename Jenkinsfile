pipeline {
    agent any
    
    environment {
        CLOUDINARY_CLOUD_NAME = credentials('CLOUDINARY_CLOUD_NAME')
        CLOUDINARY_API_KEY = credentials('CLOUDINARY_API_KEY')
        CLOUDINARY_API_SECRET = credentials('CLOUDINARY_API_SECRET')
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/thatisawais/gaming-store.git'
            }
        }
        
        stage('Create .env file') {
            steps {
                script {
                    sh '''
                        cat > .env << EOF
CLOUDINARY_CLOUD_NAME=${CLOUDINARY_CLOUD_NAME}
CLOUDINARY_API_KEY=${CLOUDINARY_API_KEY}
CLOUDINARY_API_SECRET=${CLOUDINARY_API_SECRET}
EOF
                    '''
                    sh 'ls -la' // Remove or hide in production
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose down && docker-compose -p ecommerce_pipeline -f docker-compose.yml up -d --build'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}
