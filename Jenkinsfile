pipeline {
    agent any

    environment {
        // IP Server Kantor (VPS Anda)
        SERVER_IP = '103.181.142.253'
        // Folder Project di Server
        REMOTE_DIR = '/home/jawaracode-core/proyek-kantor'
    }

    stages {
        stage('Deploy to Office Server') {
            steps {
                echo '=== MULAI DEPLOYMENT ==='
                withCredentials([sshUserPrivateKey(credentialsId: 'vps-ssh-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    script {
                        def remoteCmd = """
                            echo "--- 1. Masuk Folder Proyek ---"
                            cd ${REMOTE_DIR}
                            
                            echo "--- 2. Git Pull (Update Kodingan) ---"
                            git pull origin main
                            
                            echo "--- 3. Docker Compose Up (Restart Service) ---"
                            docker compose up -d --build
                            
                            echo "--- 4. Cek Status ---"
                            docker compose ps
                        """
                        
                        // Eksekusi Remote SSH
                        sh "ssh -i $SSH_KEY -o StrictHostKeyChecking=no $SSH_USER@${SERVER_IP} '${remoteCmd}'"
                    }
                }
            }
        }
    }
}