pipeline {
    agent any

    stages {
        stage('Deploy HAProxy Config') {
            steps {
                sh '''
                    # 1. Backup file cu (co timestamp)
                    sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak-$(date +%Y%m%d-%H%M%S) || true

                    # 2. Copy file moi tu repo vao dung cho 
                    sudo cp ${WORKSPACE}/configs/haproxy.cfg /etc/haproxy/haproxy.cfg

                    # 3. Check syntax
                    if ! sudo haproxy -c -f /etc/haproxy/haproxy.cfg 2>&1 | grep -q "Configuration file is valid"; then
                        echo "Config loi syntax!"
                        sudo haproxy -c -f /etc/haproxy/haproxy.cfg 2>&1 || true
                        exit 1
                    fi

                    # 4. Reload HAProxy (0 downtime)
                    sudo systemctl reload haproxy
                    echo "Deploy thành công + reload HAProxy!"
                '''
            }
        }
    }

    post {
        success { echo 'HAProxy config đa duoc cap nhat thanh cong!' }
        failure { echo 'Deploy that bai – da giu nguyen config cu' }
    }
}
