pipeline {
  agent any
  stages {
    stage('Check Out Code') {
      steps {
        git(url: 'https://github.com/avishai201/spring-boot-examples.git', branch: 'Avishai_sol', changelog: true)
      }
    }

    stage('Maven Compile') {
      steps {
        sh 'mvn build'
      }
    }

  }
}