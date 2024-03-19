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
	    stage("Docker Build and Push") {
	        when {
				expression { params.action == 'create' }
			}
	        steps {
//	            dir("${params.AppName}") {
	                dockerBuild ( "${params.ImageName}", "${params.docker_repo}" )
//	            }
	        }
	    }
		stage("Ansible Setup") {
	        when {
				expression { params.action == 'create' }
			}
	        steps {
	          //  sh 'ansible-playbook ${WORKSPACE}/kubernetes-configmap-reload/server_setup.yml'
			    sh 'ansible-playbook ${WORKSPACE}/server_setup.yml'
			}
		}
	    stage("Create deployment") {
			when {
				expression { params.action == 'create' }
			}
	        steps {
	            sh 'echo ${WORKSPACE}'
	            sh 'kubectl create -f ${WORKSPACE}/kubernetes-configmap.yml'
	        }
	    }
    }
}
