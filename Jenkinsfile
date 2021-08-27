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

    stage('Increment the pom') {
      steps {
        sh '''cd spring-boot-package-war
mvn versions:set -DnewVersion= $Build_ID

println("Getting commit id and latest Version")
_lastCommit = sh script: "git log | head -1 | awk \'{print \\$2}\' | cut -c1-6", returnStdout: true
_latestVersion = sh script: "git branch -r | cut -d \'/\' -f2 | grep 0. | sort -r | head -1", returnStdout: true
println("BuildID Version seen is ${BUILD_ID}")

'''
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(channel: 'int-project', message: ' "${env.JOB_NAME} #${env.BUILD_NUMBER} - Started By Avishai (${env.BUILD_URL})"', notifyCommitters: true, token: 'WBoniVFZfbAefgOzyMSEscli')
      }
    }

  }
}