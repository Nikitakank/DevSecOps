pipeline {
    agent any

    stages {
        stage('Deploy WAR on Server') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'github-creds',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_TOKEN'
                    ),
                    usernamePassword(
                        credentialsId: 'technohertz-creds',
                        usernameVariable: 'SSH_USER',
                        passwordVariable: 'SSH_PASS'
                    )
                ]) {
                    script {
                        echo "Starting remote deployment..."

                        sshCommand remote: [
                            name: "TechnohertzServer",
                            host: "148.72.215.184",
                            user: SSH_USER,
                            password: SSH_PASS,
                            allowAnyHosts: true
                        ], command: """
                            set -e
                            bash /home/technohertz/War/Demo/deploy_demo.sh DevSecOps "$GIT_USER" "$GIT_TOKEN"
                        """
                    }
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

