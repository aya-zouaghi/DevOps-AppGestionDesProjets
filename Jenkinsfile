pipeline {
    agent any

    stages {

        stage('Build Backend Docker') {
            steps {
                sh 'cd backend && mvn clean package -DskipTests'
                sh 'docker build -t devops-backend:1.0 ./backend'
            }
        }

        stage('Build Frontend Docker') {
            steps {
                sh 'docker build -t devops-frontend:1.0 ./frontend'
            }
        }

        stage('Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }

    }
}
