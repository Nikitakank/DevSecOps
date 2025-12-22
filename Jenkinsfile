pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'git-creds'
        SERVER_CREDENTIALS = 'technohertz-creds'
        SERVER_USER = 'technohertz'                    // your server username
        SERVER_HOST = '148.72.215.184'         // your server IP
        REMOTE_DIR = '/home/technohertz/War/Demo'     // directory where code & deploy script exist
    }

    stages {
        stage('Update & Deploy on Server') {
            steps {
                sshagent(['technohertz-creds']) {
                    // Run all actions directly on server
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
