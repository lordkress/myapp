#!groovy
def label = "mypod-${UUID.randomUUID().toString()}"
def appName = 'myapp'
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"


pipeline {
  agent none
     node {
          stages {
              stage('Docker Build') {
			      steps {
                     sh 'echo Hello World!'
                    }
				}
			}
        }
    }