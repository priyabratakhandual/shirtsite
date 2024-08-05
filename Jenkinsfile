pipeline {
    agent any

    environment {
        SERVER_USER = 'ubuntu'
        SERVER_IP = '13.232.62.149'
        DEPLOY_DIR = '/var/www/html/'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/priyabratakhandual/shirtsite.git'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: '8e7f6548-61aa-45d7-9f3f-5aba166533e6', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                    mkdir -p ~/.ssh
                    ssh-keyscan -H ${SERVER_IP} >> ~/.ssh/known_hosts
                    scp -i ${SSH_KEY} -r * ${SERVER_USER}@${SERVER_IP}:${DEPLOY_DIR}
                    '''
                }
            }
        }

        stage('Restart NGINX') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: '8e7f6548-61aa-45d7-9f3f-5aba166533e6', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                    ssh -i ${SSH_KEY} ${SERVER_USER}@${SERVER_IP} "sudo systemctl restart nginx"
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

