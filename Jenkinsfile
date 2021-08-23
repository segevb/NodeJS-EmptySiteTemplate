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
        cleanWs(cleanWhenSuccess: true, cleanWhenAborted: true, cleanWhenFailure: true)
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
            sh '''tar -czvf node.tar.gz .
'''
          }
        }

        stage('Notify Slack') {
          steps {
            slackSend(attachments: 'joblog', channel: 'my_notifier', failOnError: true, color: '#3EA652', blocks: '"${env.JOB_NAME} #${env.BUILD_NUMBER} - " + buildStatus + " Started By ${env.BUILD_USER} (${env.BUILD_URL})")')
          }
        }

      }
    }

    stage('Publish the Archive') {
      steps {
        archiveArtifacts 'node.tar.gz'
      }
    }

  }
}