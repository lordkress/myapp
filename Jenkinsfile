def label = "mypod-${UUID.randomUUID().toString()}"
def appName = 'myapp'
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"

podTemplate(label: label, containers: [
    containerTemplate(name: 'docker', image: 'gcr.io/cloud-builders/docker:latest', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kube', image: 'gcr.io/cloud-builders/kubectl:latest', ttyEnabled: true, command: 'cat')
  ]) 

{
node(label) {
      stage ('Build image') {
		      container('docker') {
				  sh("docker build -t ${appName} .")
                }
            }
        }  
    }
}