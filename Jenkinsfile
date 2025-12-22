pipeline {
    agent any

    stages {

        stage('Trigger Deployment on Server') {
            steps {
                echo 'Triggering deployment ONLY on server...'

                bat '''
                ssh -o StrictHostKeyChecking=no technohertz@148.72.215.184 ^
                "cd /home/technohertz/War/Demo && \
                 chmod +x deploy_demo.sh && \
                 ./deploy_demo.sh"
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment triggered successfully on server'
        }
        failure {
            echo 'Deployment failed — check deploy_demo.sh logs on server'
        }
    }
}
