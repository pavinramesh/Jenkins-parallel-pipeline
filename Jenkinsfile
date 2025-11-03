pipeline {
  agent any
  options { timestamps() }
  environment { APP_NAME = 'payment-gateway' }

  stages {
    stage('Show Branch') {
      steps {
        echo "Building branch: ${env.BRANCH_NAME}"
        bat 'echo Hello from %BRANCH_NAME% (Windows shell)'
      }
    }
    stage('Build & Test (Parallel)') {
      parallel {
        stage('Lint') {
          steps { bat 'echo Linting... & timeout /t 5 >nul' }
        }
        stage('Unit Tests') {
          steps { bat 'echo Running tests... & timeout /t 5 >nul' }
        }
        stage('Docker Build (Mock)') {
          steps { bat 'echo docker build -t demo/%APP_NAME%:%BRANCH_NAME% . & timeout /t 5 >nul' }
        }
      }
    }
  }
  post {
    success { echo "✅ Done for branch ${env.BRANCH_NAME}" }
    failure { echo "❌ Failed for branch ${env.BRANCH_NAME}" }
  }
}
