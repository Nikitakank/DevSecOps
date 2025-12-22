pipeline {
    agent any

    environment {
        SERVER_HOST = "148.72.215.184"
        SSH_CRED    = "technohertz-creds"
        GIT_CRED    = "git-creds"
        REPO_NAME   = "DevSecOps"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Nikitakank/DevSecOps.git',
                        credentialsId: "${GIT_CRED}"
                    ]]
                ])
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
                    sshCommand remote: [
                        name: "TechnohertzServer",
                        host: "${SERVER_HOST}",
                        user: "technohertz",
                        credentialsId: "${SSH_CRED}",
                        allowAnyHosts: true
                    ], command: """
                        bash /home/technohertz/War/Demo/deploy_demo.sh \
                        DevSecOps \
                        ${GIT_USER} \
                        ${GIT_TOKEN}
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
            echo "WAR deployment FAILED — check deploy_demo.sh logs on server"
        }
    }
}
