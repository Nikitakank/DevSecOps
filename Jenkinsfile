pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main',
                    url: 'https://github.com/Nikitakank/DevSecOps',
                    credentialsId: 'git-creds'
            }
        }

        stage('Deploy WAR on Server') {
            steps {
                echo 'Starting deployment on 148.72.215.184...'
                withCredentials([usernamePassword(
                    credentialsId: 'technohertz-creds', 
                    usernameVariable: 'technohertz', 
                    passwordVariable: 'AJSEQCp#wv6%')]) {
                    
                    sshCommand remote: [
                        name: 'technohertz-server',  // REQUIRED
                        host: '148.72.215.184',
                        user: "${USERNAME}",
                        password: "${PASSWORD}",
                        allowAnyHosts: true
                    ], command: "bash /home/technohertz/War/Demo/deploy_demo.sh DevSecOps"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check server logs!'
        }
    }
}
