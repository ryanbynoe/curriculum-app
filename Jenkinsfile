pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/ryanbynoe/curriculum-app', branch: 'main')
      }
    }

    stage('log') {
      parallel {
        stage('log') {
          steps {
            powershell 'dir'
          }
        }

        stage('Front-End Unit Tests') {
          steps {
            bat '@echo off REM Curriculum Front-end Test Script for Windows  cd curriculum-front && (     call npm i     if %ERRORLEVEL% neq 0 exit /b %ERRORLEVEL%     call npm run test:unit     if %ERRORLEVEL% neq 0 exit /b %ERRORLEVEL% )'
          }
        }

      }
    }

  }
}