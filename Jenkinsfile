pipeline {
    agent any

    environment {
        REMOTE_USER = 'ubuntu'
        REMOTE_HOST = '172.31.76.217'
        REMOTE_PATH = '/var/www/html'
        SSH_KEY = credentials('nginx-user')  // Jenkins Credentials ID
    }

    stages {

        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Tech-devops18/devops.git', branch: 'main'
            }
        }

        stage('Clean Remote Nginx Directory') {
            steps {
                script {
                    sh """
                    ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} 'sudo rm -rf ${REMOTE_PATH}/*'
                    """
                }
            }
        }

        stage('Copy Files to Nginx Server') {
            steps {
                script {
                    sh """
                    scp -i ${SSH_KEY} -o StrictHostKeyChecking=no -r ./index.html ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/
                    """
                }
            }
        }

        stage('Reload Nginx') {
            steps {
                sh """
                ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} 'sudo systemctl reload nginx'
                """
            }
        }
    }

    post {
        success { echo "Deployment successful!" }
        failure { echo "Deployment failed." }
    }
}
