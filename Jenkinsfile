pipeline {
    agent any

    environment {
        SERVER_IP = '103.181.142.253'
        REMOTE_DIR = '/home/jawaracode-core/project/proyek-kantor'
    }

    stages {
        stage('Manual Approval') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        input message: 'Apakah Anda yakin ingin deploy update ini ke VPS Production?', ok: 'Ya, Deploy Sekarang!'
                    }
                }
            }
        }
        
        stage('Deploy to Office Server') {
            steps {
                echo '=== IZIN DITERIMA. MEMULAI DEPLOYMENT'
                withCredentials([sshUserPrivateKey(credentialsId: 'vps-ssh-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    script {
                        def remoteCmd = """
                            echo "--- 1. Masuk Folder Proyek ---"
                            cd ${REMOTE_DIR}
                            
                            echo "--- 2. Git Pull (Update Kodingan) ---"
                            git pull origin main
                            
                            echo "--- 3. Docker Compose Up (Restart Service) ---"
                            docker compose up -d --build --force-recreate
                            
                            echo "--- 4. Cek Status ---"
                            docker compose ps
                        """
                        
                        sh "ssh -i $SSH_KEY -o StrictHostKeyChecking=no $SSH_USER@${SERVER_IP} '${remoteCmd}'"
                    }
                }
            }
        }
    }
}