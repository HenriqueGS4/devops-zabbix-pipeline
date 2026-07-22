cat << 'EOF' > Jenkinsfile
pipeline {
    agent any

    stages {
        stage('Enviar Arquivos para VM Aplicação') {
            steps {
                script {
                    sh '''
                        # Cria o diretório remoto e envia o index.html + Dockerfile via SCP
                        ssh -o StrictHostKeyChecking=no henrique@192.168.18.57 "mkdir -p /home/henrique/app"
                        scp -o StrictHostKeyChecking=no index.html Dockerfile henrique@192.168.18.57:/home/henrique/app/
                    '''
                }
            }
        }

        stage('Build e Deploy Remoto') {
            steps {
                script {
                    sh '''
                        ssh -o StrictHostKeyChecking=no henrique@192.168.18.57 "
                            cd /home/henrique/app
                            
                            # Para e remove o container antigo se existir
                            docker stop app-piloto 2>/dev/null || true
                            docker rm app-piloto 2>/dev/null || true
                            
                            # Constroi a imagem personalizada usando o index.html atualizado
                            docker build -t app-piloto:v1 .
                            
                            # Sobe apenas o container com a sua imagem personalizada
                            docker run -d --name app-piloto -p 8081:80 app-piloto:v1
                        "
                    '''
                }
            }
        }
    }
}
EOF
