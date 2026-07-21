cat << 'EOF' > Jenkinsfile
pipeline {
    agent any

    stages {
        stage('Deploy Remoto via SSH') {
            steps {
                script {
                    sh '''
                        ssh -o StrictHostKeyChecking=no henrique@192.168.18.57 "
                            # Para e remove container antigo se existir
                            docker stop app-piloto 2>/dev/null || true
                            docker rm app-piloto 2>/dev/null || true
                            
                            # Sobe o container atualizado
                            docker run -d --name app-piloto -p 8081:80 nginx:alpine
                        "
                    '''
                }
            }
        }
    }
}
EOF
