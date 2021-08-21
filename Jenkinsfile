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
      parallel {
        stage('Maven Compile') {
          steps {
            sh '''cd spring-boot-package-war
mvn compile'''
          }
        }

        stage('Echo Parameters') {
          steps {
            sh '''echo BUILD_ID = $BUILD_ID
echo BUILD_TAG = $BUILD_NUMBER
echo BUILD_TAG = $BUILD_TAG
echo BUILD_URL = $BUILD_URL
echo EXECUTOR_NUMBER = $EXECUTOR_NUMBER
echo JENKINS_URL = $JENKINS_URL
'''
          }
        }

      }
    }

    stage('Maven Test + Build ID') {
      steps {
        sh '''cd spring-boot-package-war
mvn test'''
        sh '''cd spring-boot-package-war
mvn versions:set versions:commit -DnewVersion= $BUILD_ID
'''
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