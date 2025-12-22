pipeline {
    agent any

    options {
        timeout(time: 5, unit: 'MINUTES')
    }

    stages {
        stage('Trigger Deployment on Server') {
            steps {
                echo 'Triggering deployment on server (non-interactive)...'

                bat '''
                ssh -o BatchMode=yes -o ConnectTimeout=15 technohertz@148.72.215.184 ^
                "cd /home/technohertz/War/Demo && chmod +x deploy_demo.sh && ./deploy_demo.sh"
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully'
        }
        failure {
            echo 'Deployment failed or SSH authentication blocked'
        }
    }
}
