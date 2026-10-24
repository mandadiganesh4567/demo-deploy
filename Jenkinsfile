pipeline {
    agent { label 'AGENT-1' }

    stages {
        stage('Deploy') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                        sh '''
                            set -e

                            aws eks update-kubeconfig \
                                --region us-east-1 \
                                --name roboshop-dev

                            echo "=== Jenkins AWS identity ==="
                            aws sts get-caller-identity

                            echo "=== Check EKS access ==="
                            kubectl get nodes

                            helm upgrade --install demo-app ./Charts \
                                -f ./Charts/values.yaml \
                                --namespace demo-app \
                                --wait \
                                --timeout=5m
                        '''
                    }
                }
            }
        }
    }
}