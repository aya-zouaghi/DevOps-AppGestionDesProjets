pipeline {
    agent any

    stages {

        stage('Compile Backend') {
            steps {
                sh 'cd backend && mvn compile'
            }
        }

        stage('Start MySQL') {
            steps {
                sh 'docker compose up -d db'
                sh 'sleep 10'
            }
        }

        stage('Test Backend') {
            steps {
                sh 'cd backend && mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                        sh 'cd backend && mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Package') {
            steps {
                sh 'cd backend && mvn clean package -DskipTests'
            }
        }

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

        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    sh '''
                        echo "$DOCKERHUB_TOKEN" | docker login -u "$DOCKERHUB_USER" --password-stdin
                        docker tag devops-backend:1.0 "$DOCKERHUB_USER/devops-backend:1.0"
                        docker tag devops-frontend:1.0 "$DOCKERHUB_USER/devops-frontend:1.0"
                        docker push "$DOCKERHUB_USER/devops-backend:1.0"
                        docker push "$DOCKERHUB_USER/devops-frontend:1.0"
                        docker logout
                    '''
                }
            }
        }

        stage('Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
