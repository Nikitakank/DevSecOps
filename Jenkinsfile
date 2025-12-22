pipeline {
    agent any

    environment {
        REPO_NAME = 'DevSecOps'
        GIT_USER = 'nikitakank'
        GIT_TOKEN = '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' 
        BASE_DIR = "/home/technohertz/War/Demo"
        REPO_DIR = "${BASE_DIR}/repo"
        FINAL_WAR = "${BASE_DIR}/Demo.war"
        JAVA_HOME = '/usr/lib/jvm/java-17-oracle-x64'
        PATH = "${JAVA_HOME}/bin:/usr/local/maven/bin:${env.PATH}"
    }

    options {
        timeout(time: 10, unit: 'MINUTES') // Set reasonable timeout
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: "https://github.com/${GIT_USER}/${REPO_NAME}.git",
                        credentialsId: 'git-creds' // Your Git credentials in Jenkins
                    ]]
                ])
            }
        }

        stage('Build WAR with Maven') {
            steps {
                sh '''
                    echo "JAVA_HOME=${JAVA_HOME}"
                    java -version
                    mvn -version
                    mvn clean package
                '''
            }
        }

        stage('Deploy to Technohertz Server') {
            steps {
                echo "Triggering deployment on Technohertz server..."
                sshagent(['technohertz-creds']) {  // Use your SSH credentials ID
                    sh """
                        ssh -o StrictHostKeyChecking=no technohertz@148.72.215.184 \\
                        "cd ${BASE_DIR} && chmod +x deploy_demo.sh && ./deploy_demo.sh ${GIT_TOKEN}"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment SUCCESSFUL. WAR location: ${FINAL_WAR}"
        }
        failure {
            echo "Deployment FAILED. Check Jenkins logs and deploy_demo.sh output on server."
        }
    }
}
