pipeline {
    agent any

    environment {
        SERVER_USER = 'technohertz'
        SERVER_IP   = '148.72.215.184'
        GIT_TOKEN   = credentials('git-creds')
        REPO_NAME   = 'DevSecOps'
        REPO_USER   = 'nikitakank'
    }

    stages {
        stage('Trigger Deployment on Server') {
            steps {
                echo "Triggering deployment on Linux server..."
                withCredentials([usernamePassword(credentialsId: 'technohertz-creds', 
                                usernameVariable: 'SSH_USER', 
                                passwordVariable: 'SSH_PASS')]) {
                    powershell """
                    ssh $env:SSH_USER@${SERVER_IP} "cd /home/technohertz/War/Demo; bash deploy_demo.sh ${REPO_NAME} ${REPO_USER} ${GIT_TOKEN}"
                    """
                }
            }
        }
    }

    post {
        success { echo "Deployment SUCCESSFUL!" }
        failure { echo "Deployment FAILED. Check server logs." }
    }
}
