pipeline {
    agent { label 'AGENT-1' }

    stages {
        stage('Deploy') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                        sh """
                            set -e

                            aws eks update-kubeconfig \
                                --region us-east-1 \
                                --name roboshop-dev

                            kubectl get nodes

                            helm upgrade --install demo-app ./Charts \
                                -f ./Charts/values.yaml \
                                --namespace demo-app \
                                --set-string deployment.image.tag=${appVersion} \
                                --atomic --wait \
                                --timeout=5m
                        """
                    }
                }
            }
        }
    }
}