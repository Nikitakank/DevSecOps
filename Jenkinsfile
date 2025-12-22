pipeline {
    agent any

    stages {
        stage('Deploy to Server') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'technohertz-creds',
                        transfers: [
                            sshTransfer(
                                sourceFiles: '',
                                execCommand: """
                                    cd /home/technohertz/War/Demo &&
                                    git reset --hard &&
                                    git clean -fd &&
                                    git pull origin main &&
                                    bash deploy_demo.sh
                                """
                            )
                        ],
                        verbose: true
                    )
                ])
            }
        }
    }

    post {
        success { echo 'Deployment Successful!' }
        failure { echo 'Deployment Failed!' }
    }
}
