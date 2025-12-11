pipeline {
  agent any
  environment {
    EC2_USER = 'ubuntu' // change if different
    EC2_HOST = '' // set your EC2 public IP or hostname here or use Jenkins credential
    SSH_CREDENTIALS_ID = 'ec2-ssh-key' // Jenkins credential id for SSH private key
    REMOTE_APP_DIR = '/home/ubuntu/flask-app'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }  


    stage('Build Docker Image') {
      steps {
         sh 'docker build -t flask-jenkins-app .'
      }
    }


    stage('Save Image') {
      steps {
        // Save image tar to copy to remote host
        sh 'docker save flask-jenkins-app -o flask-app.tar'
      }
    }


    stage('Copy & Deploy to EC2') {
      steps {
        // Copy files and load image on remote EC2 then run
        sshagent (credentials: [env.SSH_CREDENTIALS_ID]) {
          sh '''
          scp -o StrictHostKeyChecking=no flask-app.tar ${EC2_USER}@${EC2_HOST}:${REMOTE_APP_DIR}/flask-app.tar
          scp -o StrictHostKeyChecking=no start_service.sh ${EC2_USER}@${EC2_HOST}:${REMOTE_APP_DIR}/start_service.sh
          ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'cd ${REMOTE_APP_DIR} && docker load -i flask-app.tar && chmod +x start_service.sh && ./start_service.sh'
          '''
        }
      }
    }  
  }

  post {
    always {
      cleanWs()
    }
  }
}
