pipeline {
    agent any

    environment {
        APP_NAME = "DevSecOps"
    }

    stages {
        stage('Checkout Code') {
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
                    )
                ]) {
                    script {
                        // Define remote server properly
                        def remote = [
                            name: 'technohertz',
                            host: '148.72.215.184',
                            user: SSH_USER,
                            password: SSH_PASS,
                            allowAnyHosts: true
                        ]

                        echo "Starting deployment on ${remote.host}..."

                        // Execute deploy script on remote server
                        sshCommand remote: remote, command: """
                            set -e
                            bash /home/technohertz/War/Demo/deploy_demo.sh ${APP_NAME}
                        """

                        echo "Deployment completed successfully."
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "Deployment failed. Check server logs."
        }
    }
}
