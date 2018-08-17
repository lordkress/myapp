def appName = 'myapp'
def imageTag = "https://github.com/lordkress/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"

pipeline {
    agent {
	kubernetes {
      label 'my-app'
      defaultContainer 'jnlp'
      yaml """
	  apiVersion: v1
	  kind: Pod
	  metadata:
      labels:
		component: ci
	spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
    }
	
	stages {
	stage ('Build image') {
		steps {
        sh("docker build -t ${imageTag} .")
		}
	}

	stage ('Push image to registry'){
		steps {
        sh("gcloud docker -- push ${imageTag}")
		}
	}
	
	stage ('Deploy Application') {
		steps {
	// Roll out to production
	// Change deployed image in canary to the one we just built
	//sh("sed -i.bak 's#gcr.io/cloud-solutions-images/gceme:1.0.0#${imageTag}#' ./k8s/production/*.yaml")
	sh("kubectl --namespace=production apply -f k8s/services/")
	sh("kubectl --namespace=production apply -f k8s/production/")
	sh("echo http://`kubectl --namespace=production get service/${appName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${appName}")}
	}
	}
}
