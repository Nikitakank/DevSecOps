pipeline {
    agent any

    options {
        timeout(time: 5, unit: 'MINUTES')
    }

    stages {
        stage('Trigger Deployment on Server') {
            steps {
                echo 'Triggering deployment on Technohertz server...'

                bat '''
                ssh -o BatchMode=yes -o StrictHostKeyChecking=no technohertz@148.72.215.184 ^
                "cd /home/technohertz/War/Demo && chmod +x deploy_demo.sh && ./deploy_demo.sh"
                '''
            }
        }
    }

    post {
        success {
            echo 'WAR deployment completed successfully'
        }
        failure {
            echo 'WAR deployment failed — SSH authentication issue'
        }
    }
}
