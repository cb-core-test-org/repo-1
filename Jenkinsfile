pipeline {
  agent {
    kubernetes {
      label 'bb-server-testing'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: golang
    image: golang:1.11.0-alpine
    command: ['cat']
    tty: true
  - name: curl
    image: appropriate/curl:latest
    command: ['cat']
    tty: true
"""
    }
  }
  stages {
    stage('Handle Pull Requests') {
      when {
        /* branch 'PR-*' */
        changeRequest()
      }
      environment {
        BB_SERVER_JENKINS_CREDS = credentials('bb-server-jenkins-user-token')
        APPROVAL_URL = ''
      }
      steps {
        script {
            APPROVAL_URL = CHANGE_URL.replace("/projects/","/rest/api/1.0/projects/").replace("/overview", "/approve")
        }
        container('curl') {
          sh "echo \"Testing the $APPROVAL_URL\""
        }
        container('curl') {
          sh "curl -u \"$BB_SERVER_JENKINS_CREDS_USR:$BB_SERVER_JENKINS_CREDS_PSW\" -X DELETE -H \"Content-Type: application/json\" \"$APPROVAL_URL\""
        }
        container('golang') {
          sh 'env | sort'
          sh 'go version'
        }
        container('curl') {
          sh "curl -u \"$BB_SERVER_JENKINS_CREDS_USR:$BB_SERVER_JENKINS_CREDS_PSW\" -X POST -H \"Content-Type: application/json\" \"$APPROVAL_URL\""
        }
      }
    }
    stage('Handle master') {
      when {
        branch 'master'
      }
      steps {
        container('golang') {
          sh 'env | sort'
          sh 'go version'
          sh 'echo pr test'
          sh 'echo pr2 test'
          sh 'echo pr3 test'
          sh 'echo pr4 test'
          sh 'echo pr4 test'
          sh 'echo pr6 test'
        }
      }
    }
  }
}
