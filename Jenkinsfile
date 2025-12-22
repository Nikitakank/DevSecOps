pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'git-creds'
        SERVER_CREDENTIALS = 'technohertz-creds'
        SERVER_USER = 'technohertz'      
        SERVER_HOST = '148.72.215.184' 
        REMOTE_DIR = '/home/technohertz/War/Demo' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: "${GIT_CREDENTIALS}", url: 'https://github.com/Nikitakank/DevSecOps'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(['technohertz-creds']) {
                    sh """
                    scp target/*.war ${SERVER_USER}@${SERVER_HOST}:${REMOTE_DIR}/
                    scp -r * ${SERVER_USER}@${SERVER_HOST}:${REMOTE_DIR}/
                    """
                }
            }
        }

        stage('Restart Server (Optional)') {
            steps {
                sshagent(['technohertz-creds']) {
                    sh "ssh ${SERVER_USER}@${SERVER_HOST} 'bash ${REMOTE_DIR}/deploy_demo.sh'"
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
