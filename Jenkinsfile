pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/ryanbynoe/curriculum-app', branch: 'main')
      }
    }

    stage('log') {
      steps {
        powershell 'dir'
      }
    }

  }
}