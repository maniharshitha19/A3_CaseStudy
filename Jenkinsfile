pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "Pulling latest code from Git"
                checkout scm
            }
        }


        stage('Selenium Tests with pytest') {
    steps {
        echo 'Running Selenium Tests with pytest'

        bat 'python -m pip install -r requirements.txt'
        bat 'start /B python app.py'
        bat 'ping 127.0.0.1 -n 5 > nul'

        bat 'python -m pytest -v'
    }
}

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image petrescue:v1 "
                bat "docker build -t petrescue:v1 ."
            }
        }

        stage('Tag Docker Image') {
            steps {
                echo "Tagging petrescue:v1 as maniharshitha19/pet-rescue-app:new"
                bat "docker tag petrescue:v1 maniharshitha19/pet-rescue-app:new"
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
                echo "Pushing maniharshitha19/pet-rescue-app:new to Docker Hub..."
                bat "docker push maniharshitha19/pet-rescue-app:new"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes..."
                bat "kubectl apply -f deployment.yaml --validate=false"
                bat "kubectl apply -f service.yaml"
                bat "kubectl rollout restart deployment/petrescue-deployment"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully! Deployed maniharshitha19/pet-rescue-app:new"
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}