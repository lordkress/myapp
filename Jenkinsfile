#!groovy
def label = "mypod-${UUID.randomUUID().toString()}"
def appName = 'myapp'
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"


pipeline {
  agent none
     stages {
       stage('Docker Build') {
         agent {
	            docker {
                   steps {
                      sh 'docker build -t ${appName} .'
                    }    
	            }
            }
        }
    }
}