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

        stage('Fix Docker Permissions') {
            steps {
                script {
                    // Check if running as root, if not use sudo for docker commands
                    def isRoot = sh(script: 'id -u', returnStdout: true).trim() == '0'
                    echo "Running as root: ${isRoot}"
                    
                    // Add jenkins user to docker group for this session or use sudo
                    if (!isRoot) {
                        sh 'sudo usermod -aG docker jenkins || echo "Could not add jenkins to docker group, will use sudo"'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use sudo if jenkins user doesn't have direct access to docker socket
                    def canAccessDocker = sh(script: 'docker info >/dev/null 2>&1', returnStatus: true) == 0
                    
                    if (canAccessDocker) {
                        sh 'docker-compose -p ecommerce_pipeline -f docker-compose.yml up -d --build'
                    } else {
                        sh 'sudo docker-compose -p ecommerce_pipeline -f docker-compose.yml up -d --build'
                    }
                }
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