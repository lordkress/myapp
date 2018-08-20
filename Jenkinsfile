def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, containers: [
    containerTemplate(name: 'gcloud', image: gcr.io/cloud-builders/gcloud:latest, ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kube', image: gcr.io/cloud-builders/kubectl:latest, ttyEnabled: true, command: 'cat')
  ]) 
  
{

      node(label) {
	    stages {
            stage ('Build image') {
                steps {
                    container('gcloud') {
                        sh("gcloud docker build -t ${imageTag} .")
                    }
                }
            }

            stage ('Push image to registry'){
                steps {
                    container('gcloud') {
					    sh("gcloud docker -- push ${imageTag}")
					}
                }
            }
	
            stage ('Deploy Application') {
	          steps{
                   container('cube') {
                     sh("kubectl --namespace=production apply -f k8s/services/")
                     sh("kubectl --namespace=production apply -f k8s/production/")
                     sh("echo http://`kubectl --namespace=production get service/${appName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${appName}")
		            }
                }
            }
        }
    }
}