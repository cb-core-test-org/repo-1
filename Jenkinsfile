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
    image: golang:1.11.0
    command: ['cat']
    tty: true
"""
    }
  }
  stages {
    stage('go version') {
      steps {
        container('default') {
          sh 'env > env.txt' 
          for (String i : readFile('env.txt').split("\r?\n")) {
              println i
          }
        }
        container('golang') {
          sh 'go version'
        }
      }
    }
  }
}
