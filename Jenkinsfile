pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Rishwanth14/8.2CDevSecOp.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          bat 'npm test'
        }
        emailext(
          subject: "Test Stage Status: ${currentBuild.currentResult}",
          body: "Test stage result: ${currentBuild.currentResult}",
          to: 'rishwanthmathi@gmail.com',
          attachLog: true
        )
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          bat 'npm audit || exit 0'
        }
        emailext(
          subject: "Security Scan Status: ${currentBuild.currentResult}",
          body: "Security scan result: ${currentBuild.currentResult}",
          to: 'rishwanthmathi@gmail.com',
          attachLog: true
        )
      }
    }
  }
}