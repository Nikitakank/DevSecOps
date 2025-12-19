pipeline {
    agent any

    stages {
        stage('Deploy WAR on Server') {
            steps {
                withCredentials([
                    string(credentialsId: 'github-creds', variable: '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'),
                    usernamePassword(
                        credentialsId: 'technohertz-creds',
                        usernameVariable: 'SSH_USER',
                        passwordVariable: 'SSH_PASS'
                    )
                ]) {
                    sshCommand(
                        remote: [
                            host: '148.72.215.184',
                            user: SSH_USER,
                            password: SSH_PASS,
                            allowAnyHosts: true
                        ],
                        command: """
                            set -e
                            echo "Starting deployment..."
                            bash /home/technohertz/War/Demo/deploy_demo.sh DevSecOps ${GIT_TOKEN}
                        """
                    )
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully"
        }
        failure {
            echo "Deployment failed. Check server logs."
        }
    }
}
