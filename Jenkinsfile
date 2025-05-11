pipeline {
    agent any

    environment {
        CLOUDINARY_CLOUD_NAME = credentials('CLOUDINARY_CLOUD_NAME')
        CLOUDINARY_API_KEY = credentials('CLOUDINARY_API_KEY')
        CLOUDINARY_API_SECRET = credentials('CLOUDINARY_API_SECRET')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/thatisawais/gaming-store.git', branch: 'main'
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
                    sh 'ls -la'
                }
            }
        }

        stage('Clean up existing containers') {
            steps {
                sh '''
                    docker-compose down --volumes --remove-orphans || true
                    docker ps -a -q --filter "name=ecommerce-app" | xargs --no-run-if-empty docker rm -f
                '''
            }
        }

        stage('Build & Run Docker Image') {
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
