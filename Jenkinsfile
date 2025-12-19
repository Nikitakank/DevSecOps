pipeline {
    agent any

    environment {
        GIT_CRED_ID = 'git-creds'        // Your Git credentials ID
        SERVER_USER = 'technohertz'      // Server username
        SERVER_PASS = 'AJSEQCp#wv6%' // Server password credential ID in Jenkins
        SERVER_HOST = '148.72.215.184'   // Server IP
        REMOTE_PATH = '/home/technohertz/War/Demo/deploy_demo.sh' // Deploy script path
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
                git branch: 'main', url: 'https://github.com/Nikitakank/DevSecOps', credentialsId: "${GIT_CRED_ID}"
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                echo "Starting deployment on ${SERVER_HOST}..."

                withCredentials([usernamePassword(credentialsId: "${SERVER_PASS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sshCommand remote: [
                        host: "${SERVER_HOST}",
                        user: "${SERVER_USER}",
                        password: "${PASSWORD}",
                        allowAnyHosts: true
                    ], command: "bash ${REMOTE_PATH} DevSecOps"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed. Check server logs!"
        }
    }
}
