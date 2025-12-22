pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Nikitakank/DevSecOps.git',
                        credentialsId: 'git-creds'
                    ]]
                ])
            }
        }

        stage('Deploy WAR on Technohertz Server') {
            steps {
                echo 'Deploying WAR on Technohertz server...'

                sh '''
                ssh -o StrictHostKeyChecking=no technohertz@148.72.215.184 << 'EOF'
                    set -e
                    chmod +x /home/technohertz/War/Demo/deploy_demo.sh
                    /home/technohertz/War/Demo/deploy_demo.sh nikitakank
                EOF
                '''
            }
        }
    }

    post {
        success {
            echo 'WAR deployment SUCCESSFUL'
        }
        failure {
            echo 'WAR deployment FAILED — check deploy_demo.sh logs on server'
        }
    }
}
