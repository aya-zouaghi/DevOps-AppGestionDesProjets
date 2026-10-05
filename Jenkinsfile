pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''echo "=========================================="
echo "Bonjour depuis Jenkins Blue Ocean !"
echo "Date système : $(date)"
echo "Numéro de build : $BUILD_NUMBER"
echo "Nom du job : $JOB_NAME"
echo "Workspace : $WORKSPACE"
echo "=========================================="'''
      }
    }

    stage('Test') {
      steps {
        sh '''echo "Exécution des tests..."
echo "Tests terminés avec succès !"'''
      }
    }

    stage('Deploy') {
      steps {
        sh '''echo "Déploiement en cours..."
echo "Application déployée avec succès !"'''
      }
    }

  }
}