pipeline {
  agent any
  options { timestamps() }
  environment { APP_NAME = 'payment-gateway' }

  stages {
    stage('Show Branch') {
      steps {
        echo "Building branch: ${env.BRANCH_NAME}"
        script {
          if (isUnix()) {
            sh 'echo Hello from ${BRANCH_NAME} (Linux shell)'
          } else {
            bat 'echo Hello from %BRANCH_NAME% (Windows shell)'
          }
        }
      }
    }

    stage('Build & Test (Parallel)') {
      parallel {
        stage('Lint') {
          steps {
            script {
              if (isUnix()) { sh 'echo Linting... && sleep 5' }
              else          { bat 'echo Linting... & timeout /t 5 >nul' }
            }
          }
        }
        stage('Unit Tests') {
          steps {
            script {
              if (isUnix()) { sh 'echo Running tests... && sleep 5' }
              else          { bat 'echo Running tests... & timeout /t 5 >nul' }
            }
          }
        }
        stage('Docker Build (Mock)') {
          steps {
            echo "Pretend docker build for ${env.APP_NAME}"
            script {
              if (isUnix()) {
                sh 'echo docker build -t demo/${APP_NAME}:${BRANCH_NAME} . && sleep 5'
              } else {
                bat 'echo docker build -t demo/%APP_NAME%:%BRANCH_NAME% . & timeout /t 5 >nul'
              }
            }
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
