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
sleep 5 && 
curl localhost:8080
if [[ "x$?" == "x0" ]];
then echo good;
else exit 1;
fi'''
      }
    }

    stage('Package Code') {
      parallel {
        stage('Package Code') {
          steps {
            sh 'tar -czvf node.tar.gz *'
          }
        }

        stage('Notify Slack') {
          steps {
            slackSend(attachments: 'joblog', channel: 'my_notifier', color: '#3EA652', blocks: 'job success')
          }
        }

      }
    }

    stage('Publish the Archive') {
      steps {
        archiveArtifacts 'node.tar.gz'
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenSuccess: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true)
      }
    }

  }
}