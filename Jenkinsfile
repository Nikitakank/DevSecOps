pipeline {
    agent any

    environment {
        SERVER_CREDENTIALS = 'technohertz-creds'
        SERVER_USER = 'technohertz'                   
        SERVER_HOST = '148.72.215.148'        
        REMOTE_DIR = '/home/technohertz/War/Demo'     
    }

    stages {
        stage('Update & Deploy on Server') {
            steps {
                sshagent(['technohertz-creds']) {
                    sh """
                    ssh ${SERVER_USER}@${SERVER_HOST} '
                        cd ${REMOTE_DIR} &&
                        git reset --hard &&
                        git clean -fd &&
                        git pull origin main &&
                        bash deploy_demo.sh
                    '
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
