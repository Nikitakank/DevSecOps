pipeline {
    agent any

    environment {
        REPO_NAME = "DevSecOps"
        DEPLOY_SCRIPT = "/home/technohertz/War/Demo/deploy_demo.sh"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy WAR on Technohertz Server') {
            steps {
                echo "Deploying WAR on remote server..."

                withCredentials([
                    usernamePassword(
                        credentialsId: 'git-creds',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_TOKEN'
                    )
                ]) {
                    sshCommand(
                        remote: [
                            name: 'TechnohertzServer',
                            host: '148.72.215.184',
                            user: 'technohertz',
                            password: env.GIT_TOKEN,
                            allowAnyHosts: true
                        ],
                        command: """
                            chmod +x ${DEPLOY_SCRIPT}
                            ${DEPLOY_SCRIPT} ${REPO_NAME}
                        """
                    )
                }
            }
        }
    }

    post {
        success {
            echo "WAR deployment SUCCESSFUL"
        }
        failure {
            echo "WAR deployment FAILED  — check deploy_demo.sh logs on server"
        }
    }
}
