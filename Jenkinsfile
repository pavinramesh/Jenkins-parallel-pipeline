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
          steps {
            bat 'echo Linting... && powershell -NoProfile -Command "Start-Sleep -Seconds 5"'
          }
        }
        stage('Unit Tests') {
          steps {
            bat 'echo Running tests... && powershell -NoProfile -Command "Start-Sleep -Seconds 5"'
          }
        }
        stage('Docker Build (Mock)') {
          steps {
            bat 'echo docker build -t demo/%APP_NAME%:%BRANCH_NAME% . && powershell -NoProfile -Command "Start-Sleep -Seconds 5"'
          }
        }
      }
    }
  }

  post {
    success { echo "✅ Done for branch ${env.BRANCH_NAME}" }
    failure { echo "❌ Failed for branch ${env.BRANCH_NAME}" }
  }
}
