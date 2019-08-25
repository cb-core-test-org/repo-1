pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
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
"""
    }
  }
  stages {
    stage('go version') {
      steps {
        container('golang') {
          sh 'env | sort'
          sh 'go version'
          sh 'echo pr test'
          sh 'echo pr2 test'
          sh 'echo pr3 test'
        }
      }
    }
  }
}
