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
					sh("docker build -t ${imageTag} .")
                }
            }

            stage ('Push image to registry'){
                container('gcloud') {
				    sh("gcloud docker -- push ${imageTag}")
                }
            }
	
            stage ('Deploy Application') {
                container('kube') {
                    sh("kubectl --namespace=production apply -f k8s/services/")
                    sh("kubectl --namespace=production apply -f k8s/production/")
                    sh("echo http://`kubectl --namespace=production get service/${appName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${appName}")
		        }
            }
        
        }
    }