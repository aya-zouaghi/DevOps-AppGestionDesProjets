pipeline {
    agent any

    stages {

stage('Compile Backend') {            steps {                sh 'cd backend && mvn compile'            }        }
stage('Test Backend') {            steps {                sh 'cd backend && mvn test'            }        }
stage('SonarQube Analysis') {            steps {                withSonarQubeEnv('SonarQube') {                    sh 'cd backend && mvn sonar:sonar'                }            }        }
stage('Quality Gate') {            steps {                timeout(time: 5, unit: 'MINUTES') {                    waitForQualityGate abortPipeline: true                }            }        }
stage('Package') {            steps {                sh 'cd backend && mvn clean package -DskipTests'            }        }
        stage('Build Backend Docker') {
            steps {
                
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
