pipeline {
    agent any

    environment {
        IMAGE_NAME = "python-api"
        CONTAINER_NAME = "python-api-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/python-api.git'
            }
        }

        stage('Build') {
            steps {
                sh 'podman build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                sh '''
                podman run --rm $IMAGE_NAME pytest -v
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                podman rm -f $CONTAINER_NAME || true
                podman run -d --name $CONTAINER_NAME -p 5001:5000 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
