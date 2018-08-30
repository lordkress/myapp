def label = "mypod-${UUID.randomUUID().toString()}"
def appName = 'my-app'
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"

podTemplate(label: label, containers: [
    containerTemplate(name: 'docker', image: 'gcr.io/cloud-builders/docker:latest', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kube', image: 'gcr.io/cloud-builders/kubectl:latest', ttyEnabled: true, command: 'cat')
  ],
 volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
) 

{
    node(label) {
        stage ('Build image') {
		        container('docker') {
				  #sh("docker build -t lordkress/${appName}:latest .")
				  sh("docker version")
                }
            }
        }  
}