pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
                git(
                    url: 'https://github.com/Nikitakank/DevSecOps',
                    branch: 'main',
                    credentialsId: 'git-creds'
                )
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                echo "Triggering deploy_demo.sh on Technohertz server..."
                
                // SSH execution using credentials stored in Jenkins
                sshCommand remote: [
                    name: "TechnohertzServer",
                    host: "148.72.215.184",
                    credentialsId: "technohertz-creds",
                    allowAnyHosts: true
                ], command: """
                    set -e
                    echo "Running deploy_demo.sh on server..."
                    bash /home/technohertz/War/Demo/deploy_demo.sh nikitakank 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
                """
            }
        }
    }

    post {
        success {
            echo "WAR build & deployment finished successfully on server."
        }
        failure {
            echo "WAR build FAILED — check server logs."
        }
    }
}
