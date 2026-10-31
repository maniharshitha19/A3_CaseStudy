pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t petrescue:v1 ."
            }
        }
        stage('Run Selenium Tests') {
            steps {
                echo "Run Selenium Tests"
                bat 'pip install -r requirements.txt'
                bat 'python app.py &'
                bat 'sleep 5'
                bat 'python -m pytest -v'
            }
        }
        stage('Docker Login') {
            steps {
                echo "Logging in to Docker Hub..."
                bat 'docker login -u maniharshitha19 -p 8328132135@mani'
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"
                bat "docker tag petrescue:v1 maniharshitha19/pet-rescue-app:new"
                bat "docker push maniharshitha19/pet-rescue-app:new"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
