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
            bat '@echo off REM Curriculum Front-end Test Script for Windows  REM Change directory cd /d "%~dp0curriculum-front" if %ERRORLEVEL% neq 0 (     echo Failed to change directory to curriculum-front     exit /b %ERRORLEVEL% )  REM Install dependencies echo Installing npm dependencies... call npm install if %ERRORLEVEL% neq 0 (     echo npm install failed     exit /b %ERRORLEVEL% )  REM Run unit tests echo Running unit tests... call npm run test:unit if %ERRORLEVEL% neq 0 (     echo Unit tests failed     exit /b %ERRORLEVEL% )  echo All tasks completed successfully. exit /b 0'
          }
        }

      }
    }

    stage('Build') {
      steps {
        powershell 'call docker_build.bat'
      }
    }

    stage('Log Into DockerHub') {
      environment {
        DOCKERHUB_USER = ''
        DOCKERHUB_PASSWORD = ''
      }
      steps {
        sh 'docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASSWORD'
      }
    }

    stage('Push') {
      steps {
        sh 'docker push fuze365/curriculum-front:latest'
      }
    }

  }
}