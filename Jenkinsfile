def label = "mypod-${UUID.randomUUID().toString()}"
def appName = 'myapp'
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"


node(label) {
      stage ('Build image') {
          sh("echo Hello World!")
        }
    }