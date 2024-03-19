@Library('jenkins-shared-library@main') _
pipeline {

  agent any
  
  parameters {
	choice(name: 'action', choices: 'create\nrollback', description: 'Create/rollback of the deployment')
    string(name: 'ImageName', description: "Name of the docker build", defaultValue: "kubernetes-configmap-reload")
	string(name: 'ImageTag', description: "Name of the docker build",defaultValue: "v1")
	string(name: 'AppName', description: "Name of the Application",defaultValue: "kubernetes-configmap-reload")
    string(name: 'docker_repo', description: "Name of docker repository",defaultValue: "srinivish")
	string(name: 'Git_URL', description: "GIT Repo URL",defaultValue: "https://github.com/srinivish/ex8_kubernetes.git")
  }
      
    stages {
        stage('Git Checkout') {
            when {
				expression { params.action == 'create' }
			}
            steps {
                gitCheckout(
                    branch: "main",
                    url: "${params.Git_URL}"
                )
            }
        }
        stage('Build Maven'){
            when {
				expression { params.action == 'create' }
			}
    		steps {
 //       		dir("${params.AppName}") {
        			sh 'mvn clean package'
 //       		}
    		}
	    }
    }
}
