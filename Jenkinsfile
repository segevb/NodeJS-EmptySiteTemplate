pipeline {
  agent {
    node {
      label 'centos7-nodejs'
    }

  }
  stages {
    stage('Check Out Code') {
      steps {
        git(url: 'https://github.com/segevb/NodeJS-EmptySiteTemplate.git', branch: 'master', changelog: true, poll: true)
      }
    }

    stage('Build & Compile') {
      steps {
        sh '''npm install
'''
      }
    }

    stage('Test Code') {
      steps {
        sh '''node server.js &
curl localhost:8080
if [[ "x$?" == "x0" ]];
then echo good;
else exit 1;
fi'''
      }
    }

  }
}