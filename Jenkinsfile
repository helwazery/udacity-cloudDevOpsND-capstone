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
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'ansible-playbook create-k8-cluster.yml'
                }
            }
        }

}
}
