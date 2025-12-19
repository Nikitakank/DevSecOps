pipeline {
    agent any

    environment {
        SERVER_HOST = "148.72.215.184"               // Your server IP
        SSH_CRED = "technohertz-creds"              // Jenkins global credentials ID
        REPO_URL = "https://github.com/Nikitakank/DevSecOps"
        BRANCH = "main"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo "Checking out code from GitHub..."
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${BRANCH}"]],
                    userRemoteConfigs: [[
                        url: "${REPO_URL}",
                        credentialsId: "${SSH_CRED}"
                    ]]
                ])
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                echo "Triggering deploy_demo.sh on Technohertz server..."
                sshCommand remote: [
                    name: "TechnohertzServer",         // Required by SSH plugin
                    host: "${SERVER_HOST}",
                    credentialsId: "${SSH_CRED}",
                    allowAnyHosts: true
                ], command: """
                    set -e
                    echo "Running deploy_demo.sh on server..."
                    bash /home/technohertz/War/Demo/deploy_demo.sh DevSecOps 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
                """
            }
        }
    }

    post {
        success {
            echo " WAR deployment SUCCESSFUL!"
        }
        failure {
            echo " WAR deployment FAILED — check server logs."
        }
    }
}
