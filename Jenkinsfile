pipeline {
  agent {
    node {
      label 'centos7-nodejs'
    }

  }
  stages {
    stage('Check Out Code') {
      steps {
        git(url: 'https://github.com/avishai201/spring-boot-examples.git', branch: 'Avishai_sol', changelog: true)
      }
    }

    stage('Maven Compile') {
      steps {
        sh '''cd spring-boot-package-war
mvn compile'''
      }
    }

    stage('Maven Test') {
      steps {
        sh '''cd spring-boot-package-war
mvn test'''
      }
    }

    stage('Maven Package') {
      steps {
        sh '''cd spring-boot-package-war
mvn clean package'''
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(message: 'Build Success - Module2', token: 'WBoniVFZfbAefgOzyMSEscli', channel: 'int-project', notifyCommitters: true)
      }
    }

  }
}