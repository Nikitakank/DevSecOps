pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Nikitakank/DevSecOps',
                    credentialsId: 'git-creds'
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                sshCommand remote: [
                    name: "TechnohertzServer",
                    host: "148.72.215.184",
                    user: "technohertz",
                    credentialsId: "technohertz-creds",
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
            echo "WAR build & copy finished on server."
        }
        failure {
            echo "WAR build FAILED — check server logs."
        }
    }
}
