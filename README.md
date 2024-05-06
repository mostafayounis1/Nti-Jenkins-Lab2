# Jenkins Project Diagram Overview

![](https://github.com/mostafayounis1/shared_library.git)

## Shared Libirary Repository
https://github.com/AliKhamed/shared_library.git

## Pipeline Overview

The Jenkins pipeline follows these stages to build, push, edite and deploy Docker images to dockerhub and an Minikube cluster:

1. **Build Docker Image:** Build docker image.

2. **Push Docker Image To DockerHub:** Push docker image to docker hub.

3. **Edit new image in deployment.yaml file:** Edit new image in deployment.yaml file.

4. **Deploy to k8s:**  Deploy image to k8s minikube cluster.


## Pipeline Steps

### Build Docker Image:

```
 stage('Build Docker image from Dockerfile in GitHub') {
            steps {
                script {
                 	
                 		buildDockerImage("${imageName}")
                      
                }
            }
        }
```



### Push Docker Image To DockerHub:

```
stage('Push image to Docker hub') {
            steps {
                script {
                 	
                 		pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                      
                }
            }
        }

```

### Edit new image in deployment.yaml file:

```
stage('Edit new image in deployment.yaml file') {
            steps {
                script { 
                	dir('k8s') {
				        editNewImage("${imageName}")
                    	}
                }
            }
        }
```
### Deploy to k8s:

```
stage('Deploy on k8s Cluster') {
            steps {
                script { 
                	dir('k8s') {
				         deployOnKubernetes("${k8sCredentialsID}", "${branchName}")
                    }
                }
            }
        }

```


### Post-Build Actions
In case of pipeline success or failure, the following messages will be displayed:
```
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
```
----
### Successfully Run  Multibranch Pipeline
![](![Screenshot 2024-05-06 180122](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/65f728b0-f71c-48bc-b19b-fc265b23fbda)




### Successfully Run Test Branch Pipeline On k8s minikube cluster
![](![testp](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/da67c8cb-8f25-46ff-aa6b-551b34aa124f)

!![test](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/817863e9-408d-4a8f-9c11-f23923e7197b)


### Successfully Run Development Branch Pipeline On k8s minikube cluster
![devp](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/dd29cfa0-7464-4eee-9c58-a2f9178cb307)
![dev](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/9ad71178-9e80-4e27-9629-e396ff6904a1)



### Successfully Run Production Branch Pipeline On k8s minikube cluster
![prodp](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/959ffe0c-a0c7-4c23-940c-d4b1460b8cc1)
![production](https://github.com/mostafayounis1/Nti-Jenkins-Lab2/assets/167571650/132db9a8-d99c-4320-b825-cff87c1194f6)

