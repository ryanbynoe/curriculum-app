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
        powershell '# Jenkins Docker Build PowerShell Script  # Function to check if a command exists function Test-Command($cmdname) {     return [bool](Get-Command -Name $cmdname -ErrorAction SilentlyContinue) }  # Display current location and directory contents Write-Host "Current location: $PWD" -ForegroundColor Cyan Write-Host "Directory contents:" -ForegroundColor Cyan Get-ChildItem | Format-Table Name, LastWriteTime, Length  # Check for curriculum-front directory if (Test-Path "curriculum-front") {     Write-Host "curriculum-front directory found." -ForegroundColor Green     Write-Host "Contents of curriculum-front:" -ForegroundColor Cyan     Get-ChildItem "curriculum-front" | Format-Table Name, LastWriteTime, Length } else {     Write-Host "curriculum-front directory not found!" -ForegroundColor Red }  # Check Docker availability if (Test-Command "docker") {     Write-Host "Docker is available. Checking Docker info:" -ForegroundColor Green     docker info     if ($LASTEXITCODE -ne 0) {         Write-Host "Error getting Docker info. Docker may not be running." -ForegroundColor Red         exit 1     } } else {     Write-Host "Docker command not found!" -ForegroundColor Red     exit 1 }  # Attempt Docker build if (Test-Path "curriculum-front\\Dockerfile") {     Write-Host "Dockerfile found. Attempting Docker build..." -ForegroundColor Green     docker build -f curriculum-front/Dockerfile .     if ($LASTEXITCODE -ne 0) {         Write-Host "Docker build failed." -ForegroundColor Red         exit 1     } } else {     Write-Host "Dockerfile not found in curriculum-front directory!" -ForegroundColor Red     exit 1 }  Write-Host "Script execution completed successfully." -ForegroundColor Green'
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