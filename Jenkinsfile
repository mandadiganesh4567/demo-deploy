pipeline {
    agent { label 'AGENT-1'} 
    stages {
        stage ('deploy') {
            steps {
                sh """
                kubectl apply -f namespace.yaml
                kubectl apply -f service.yaml
                kubectl apply -f configmap.yaml
                kubectl apply -f manifest.yaml
              """
            }
        }
    }
}