pipeline {
    agent any

    environment {
        APP_NAME = "DevSecOps"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Nikitakank/DevSecOps',
                    credentialsId: 'github-creds'
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'technohertz-creds', 
                        usernameVariable: 'SSH_USER', 
                        passwordVariable: 'SSH_PASS'
                    ),
                    usernamePassword(
                        credentialsId: 'github-creds', 
                        usernameVariable: 'GIT_USER', 
                        passwordVariable: 'GIT_TOKEN'
                    )
                ]) {
                    sshCommand remote: [
                        host: '148.72.215.184',
                        user: SSH_USER,
                        password: SSH_PASS,
                        allowAnyHosts: true
                    ], command: """
                        set -e
                        bash /home/technohertz/War/Demo/deploy_demo.sh ${APP_NAME} ${GIT_TOKEN}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
        }
        failure {
            echo "Deployment failed. Check server logs."
        }
    }
}
