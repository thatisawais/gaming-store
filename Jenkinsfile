pipeline {
    agent any
    
    environment {
        // Define environment variables for your build
        CLOUDINARY_CLOUD_NAME = credentials('CLOUDINARY_CLOUD_NAME')
        CLOUDINARY_API_KEY = credentials('CLOUDINARY_API_KEY')
        CLOUDINARY_API_SECRET = credentials('CLOUDINARY_API_SECRET')
        // Add other environment variables as needed
    }

    stages {
        stage('Clone') {
            steps {
                // Clone from main branch explicitly
                git branch: 'main', url: 'https://github.com/thatisawais/gaming-store.git'
            }
        }
        
        stage('Create .env file') {
            steps {
                script {
                    // Create .env file with required environment variables
                    sh '''
                        cat > .env << EOF
CLOUDINARY_CLOUD_NAME=${CLOUDINARY_CLOUD_NAME}
CLOUDINARY_API_KEY=${CLOUDINARY_API_KEY}
CLOUDINARY_API_SECRET=${CLOUDINARY_API_SECRET}
# Add other environment variables as needed
EOF
                    '''
                    // Optional: List directory contents to verify .env was created (remove in production)
                    sh 'ls -la'
                }
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
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}