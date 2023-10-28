pipeline {
	agent any
	environment {
       KUBECONFIG='~/.kube/kubeconfig'                            
   }
	stages {

		stage('Lint HTML') {
			steps {
				sh 'tidy -q -e ./docker/blue/*.html'
			}
		}
		
		stage('Build Docker Image blue') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-credential-udacity-devops-final', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
						docker build -t bacnguyen1005/udacity-capstone-docker-blue ./docker/blue/
					'''
				}
			}
		}

		stage('Push Image To Dockerhub blue') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-credential-udacity-devops-final', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
						docker push bacnguyen1005/udacity-capstone-docker-blue
					'''
				}
			}
		}

		stage('Build Docker Image green') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-credential-udacity-devops-final', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker build -t bacnguyen1005/udacity-capstone-docker-green ./docker/green/
					'''
				}
			}
		}

		stage('Push Image To Dockerhub green') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-credential-udacity-devops-final', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
						docker push bacnguyen1005/udacity-capstone-docker-green
					'''
				}
			}
		}

		stage('Set Current kubectl Context') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-credential-udacity-devops-final') {
                    withEnv(["KUBECONFIG=$HOME/.kube/kubeconfig"]) {
                    sh '''
						kubectl config use-context arn:aws:eks:us-east-1:490204598655:cluster/udacitycluster
					'''
                    }	
				}
			}
		}

		stage('Deploy Blue Container') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-credential-udacity-devops-final') {
                    withEnv(["KUBECONFIG=$HOME/.kube/kubeconfig"]) {
                    sh '''
						kubectl apply -f ./kubernetes-resources/blue-replication-controller.yml 
					'''
                    }	
				}
			}
		}

		stage('Deploy green container') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-credential-udacity-devops-final') {
                    withEnv(["KUBECONFIG=$HOME/.kube/kubeconfig"]) { 
					sh '''
						kubectl apply -f ./kubernetes-resources/green-replication-controller.yml 
					'''
				    }
                }
			}
		}

		stage('Create Service Pointing to Blue Replication Controller') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-credential-udacity-devops-final') {
                    withEnv(["KUBECONFIG=$HOME/.kube/kubeconfig"]) {
					sh '''
						kubectl apply -f ./kubernetes-resources/blue-service.yml 
					'''
                    }
                }
			}
		}

		stage('Approval for Redirection') {
            steps {
                input "Ready to redirect traffic to green replication controller?"
            }
        }

		stage('Create Service Pointing to Green Replication Controller') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-credential-udacity-devops-final') {
					withEnv(["KUBECONFIG=$HOME/.kube/kubeconfig"]) {
                    sh '''
						kubectl apply -f ./kubernetes-resources/green-service.yml 
					'''
                    }
                }
			}
		}

	}
}
