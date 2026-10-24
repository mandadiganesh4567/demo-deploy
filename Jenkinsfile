pipeline {
    agent { label 'AGENT-1' }

    stages {
        stage('Deploy') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                        sh '''
                            echo "=== Jenkins agent ==="
                            hostname
                            ip -4 route get 10.0.11.100

                            AGENT_IP=$(ip -4 route get 10.0.11.100 |
                            awk '{for(i=1;i<=NF;i++) if($i=="src"){print $(i+1); exit}}')

                            echo "Agent source IP: $AGENT_IP"

                            aws ec2 describe-instances \
                            --region us-east-1 \
                            --filters "Name=private-ip-address,Values=$AGENT_IP" \
                            --query 'Reservations[].Instances[].{Instance:InstanceId,IP:PrivateIpAddress,SGs:SecurityGroups[].GroupId}' \
                            --output table || true
                            set -e

                            aws eks update-kubeconfig \
                                --region us-east-1 \
                                --name roboshop-dev

                            kubectl get nodes

                            helm upgrade --install demo-app ./Charts \
                            -f ./Charts/values.yaml \
                            --namespace demo-app \
                            --atomic--wait \
                            --timeout=5m
                        '''
                    }
                }
            }
        }
    }
}