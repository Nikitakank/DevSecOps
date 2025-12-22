pipeline {
    agent any

    stages {
        stage('Build WAR on Server') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'git-creds',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_PASS'
                    ),
                    usernamePassword(
                        credentialsId: 'technohertz-creds',
                        usernameVariable: 'SSH_USER',
                        passwordVariable: 'SSH_PASS'
                    )
                ]) {
                    script {
                        sshCommand remote: [
                            name: "TechnohertzServer",
                            host: "148.72.215.184",
                            user: SSH_USER,
                            password: SSH_PASS,
                            allowAnyHosts: true
                        ], command: """
                            set -e
                            echo "Running deploy_demo.sh on server..."
                            bash /home/technohertz/War/Demo/deploy_demo.sh "nikitakank" "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""
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "WAR build & copy finished on server."
        }
        failure {
            echo "WAR build FAILED — check server logs."
        }
    }
}
