pipeline {
    agent any
    stages {
    stage ('Lint HTML page'){
            steps {
               sh 'tidy -q -e index.html'
        }
     }


    stage(' Build Kubernetes Cluster'){
            steps {
                withAWS(region:'eu-central-1', credentials:'aws-capstone') {
                    sh 'ansible-playbook create-k8-cluster.yml'
                }
            }
        }

}
}
