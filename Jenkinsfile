pipeline {
    agent any
    stages {
    stage ('Lint HTML page'){
            steps {
               sh '''
                    tidy -q -e index.html
                    hadolint Dockerfile
                  '''
        }
     }


    stage(' Build Kubernetes Cluster'){
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh 'ansible-playbook update-k8-cluster.yml'
                }
            }
        }
     stage(' Update Kubernetes Cluster config '){
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh 'ansible-playbook update-k8-cluster.yml'
                }
            }
        }

     
     stage('Build and Push Docker Image') {
	    steps {
		withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-capstone', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker build -t halaelwazery/capstone .
                                                docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                                                docker push halaelwazery/capstone
					'''
		   }
		 }
		}
     stage(' Set kubectl context/Cluster'){
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh '''
                                                  
                                                aws eks update-kubeconfig --name capstone
                                                kubectl config use-context arn:aws:eks:eu-central-1:663701914050:cluster/capstone
                                                
                       '''
                }
            }
        }

     stage('Deploy blue rollout') {
	    steps {
		withAWS(region:'eu-central-1', credentials:'aws-capstone') {
		    sh '''
						kubectl apply -f ./blue-controller.json
					'''
		   }
		 }
		}

     stage('Deploy green rollout') {
	    steps {
		withAWS(region:'eu-central-1', credentials:'aws-capstone') {
		    sh '''
						kubectl apply -f ./green-controller.json
					'''
		    }
		  }
		}

     stage('redirect the traffic to  blue rollout') {
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh '''
                                                kubectl apply -f ./blue-service.json
                                        '''
                   }
                 }
                }
     stage('traffic is now in blue rollout, redirecting to green') {
            steps {
                input "Would you like to redirect the traffic to green rollout?"
            }
        }

     stage('Redirect the traffic to  green rollout') {
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh '''
                                                kubectl apply -f ./blue-green-service.json
                                        '''
                    }
                  }
                }

}
}
