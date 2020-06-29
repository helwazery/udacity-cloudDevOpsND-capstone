pipeline {
    agent any
    stages {
    stage ('Lint HTML page'){
            steps {
               sh 'tidy -q -e index.html'
        }
     }


  #  stage(' Build Kubernetes Cluster'){
   #         steps {
    #            withAWS(region:'eu-central-1', credentials:'aws-capstone') {
     #               sh 'ansible-playbook create-k8-cluster.yml'
      #          }
      #      }
      #  }
     stage(' Update Kubernetes Cluster config '){
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh 'ansible-playbook update-k8-cluster.yml'
                }
            }
        }

     
     stage('Build Docker Image') {
	    steps {
		withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-capstone', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker build -t capstone .
					'''
				}
			}
		}

}
}
