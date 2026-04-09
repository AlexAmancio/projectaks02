pipeline {
  agent any

  options {
    timestamps()
    disableConcurrentBuilds()
    buildDiscarder(logRotator(numToKeepStr: '10'))
    skipDefaultCheckout(true)
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Verify Workspace') {
      steps {
        sh 'echo ===== Workspace ====='
        sh 'pwd'
        sh 'ls -la'
      }
    }

    stage('Build Info') {
      steps {
        script {
          writeFile file: 'build-info.txt', text: """Build: ${env.BUILD_NUMBER}
Job: ${env.JOB_NAME}
Branch: ${env.BRANCH_NAME ?: 'main/manual'}
Commit: ${env.GIT_COMMIT ?: 'no-disponible'}
"""
          archiveArtifacts artifacts: 'build-info.txt', fingerprint: true
        }
      }
    }

    stage('Success Message') {
      steps {
        echo 'Pipeline profesional ejecutado correctamente desde GitHub'
      }
    }
  }

  post {
    always {
      echo "Fin de la ejecución #${env.BUILD_NUMBER}"
    }
    success {
      echo 'Resultado: SUCCESS'
    }
    failure {
      echo 'Resultado: FAILURE'
    }
  }
}
