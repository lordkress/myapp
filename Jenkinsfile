def label = "mypod-${UUID.randomUUID().toString()}"
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
def appName = 'myapp'

podTemplate(label: label, containers: [
    containerTemplate(name: 'gcloud', image: 'gcr.io/cloud-builders/gcloud:latest', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kube', image: 'gcr.io/cloud-builders/kubectl:latest', ttyEnabled: true, command: 'cat')
  ]) 
  
   {

      node(label) {
            stage ('Build image') {
                container('gcloud') {
                    sh("gcloud docker build -t ${imageTag} .")
                }
            }

            stage ('Push image to registry'){
                container('gcloud') {
				    sh("gcloud docker -- push ${imageTag}")
                }
            }
	
            stage ('Deploy Application') {
                container('cube') {
                    sh("kubectl --namespace=production apply -f k8s/services/")
                    sh("kubectl --namespace=production apply -f k8s/production/")
                    sh("echo http://`kubectl --namespace=production get service/${appName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${appName}")
		        }
            }
        
        }
    }