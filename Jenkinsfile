@Library('shared_library')_
pipeline {
    agent any
    
    environment {
        dockerHubCredentialsID	            = 'dockerhub'  		    			      // DockerHub credentials ID.
        imageName   		            = 'mostafayounis0/nti-python-app'     			// DockerHub repo/image name.
        k8sCredentialsID	            = 'kubernetes'	                            // KubeConfig credentials ID.
	branchName                          = 'development'
    }
    
    stages {       
       
        stage('Build Docker image from Dockerfile in GitHub') {
            steps {
                script {
                 	
                 		buildDockerImage("${imageName}")
                      
                }
            }
        }
        stage('Push image to Docker hub') {
            steps {
                script {
                 	
                 		pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                      
                }
            }
        }

        stage('Edit new image in deployment.yaml file') {
            steps {
                script { 
                	dir('k8s') {
				        editNewImage("${imageName}")
			}
                }
            }
        }
        stage('Deploy on k8s Cluster') {
            steps {
                script { 
                	dir('k8s') {
				         deployOnKubernetes("${k8sCredentialsID}", "${branchName}")
                    }
                }
            }
        }
    }

    post {
        always {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline always succeeded"
        }
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
}
