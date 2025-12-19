pipeline {
    agent any

    environment {
        SERVER_HOST = "148.72.215.184"               // Your server IP
        SSH_CRED = "technohertz-creds"              // SSH credential for server
        GIT_CRED = "git-creds"                      // Git credential for GitHub
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
                        credentialsId: "${GIT_CRED}"   // <-- Git credential here
                    ]]
                ])
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                echo "Triggering deploy_demo.sh on Technohertz server..."
                sshCommand remote: [
                    name: "TechnohertzServer",       // Required by SSH plugin
                    host: "${SERVER_HOST}",
                    user: "technohertz",             // SSH username
                    credentialsId: "${SSH_CRED}",    // SSH credential here
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
            echo "WAR deployment SUCCESSFUL!"
        }
        failure {
            echo "WAR deployment FAILED — check server logs."
        }
    }
}
