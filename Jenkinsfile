pipeline {
    agent any

    environment {
        SERVER_IP     = "148.72.215.184"
        SERVER_USER   = "technohertz"
        DEPLOY_SCRIPT = "/home/technohertz/War/Demo/deploy_demo.sh"
        REPO_NAME     = "DevSecOps"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                echo "Deploying WAR on Technohertz server..."

                sshagent(credentials: ['technohertz-ssh']) {
                    bat """
                    ssh -o StrictHostKeyChecking=no %SERVER_USER%@%SERVER_IP% ^
                    "chmod +x %DEPLOY_SCRIPT% && %DEPLOY_SCRIPT% %REPO_NAME%"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "WAR deployment SUCCESSFUL"
        }
        failure {
            echo "WAR deployment FAILED— check server logs"
        }
    }
}
