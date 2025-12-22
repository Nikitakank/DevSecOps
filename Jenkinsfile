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
                                    bash deploy_demo.sh nikitakank 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
                                """,
                                removePrefix: '',
                                remoteDirectory: '/home/technohertz/War/Demo'
                            )
                        ],
                        usePromotionTimestamp: false,
                        verbose: true
                    )
                ])
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
