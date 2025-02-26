pipeline {
  agent any

  environment {
    EC2_SSH_PRIVATE_KEY = credentials('ec2_jenkins_server_key')
    EC2_HOST = '3.107.236.254'
    REPO_DIR = '/projects/gitops/testing-devops'
  }

  stages {
    stage('Checkout Code') {
        steps {
            checkout scm
        }
    }
    stage('Deploy to EC2') {
        steps {
            script {
                sshagent(['ec2_jenkins_server_key']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${env.EC2_HOST} << EOF
                        ls
                        pwd
                        cd ${env.REPO_DIR}
                        pwd
                        git pull origin main
			sudo docker rm -f $(docker ps -aq)
                        sudo docker compose up -d --build
                    << EOF
                    """
                    }
                }
            }
        }
  }
 
}

