pipeline {
    agent any
    stage(' Build Kubernetes Cluster'){
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'ansible-playbook create-k8-cluster.yml'
                }
            }
        }

}
