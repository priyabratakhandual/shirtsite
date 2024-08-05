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
                // Checkout the source code from your repository
                git 'https://github.com/priyabratakhandual/shirtsite.git'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy static files to the server
                sh '''
                scp -r * ${SERVER_USER}@${SERVER_IP}:${DEPLOY_DIR}
                '''
            }
        }

        stage('Restart NGINX') {
            steps {
                // Restart NGINX to apply changes if necessary
                sh '''
                ssh ${SERVER_USER}@${SERVER_IP} "sudo systemctl restart nginx"
                '''
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}
