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
                    image 'docker:latest'                       
	            }
            }
			  steps {
                     sh 'echo Hello World!'
                    }
        }
    }
}