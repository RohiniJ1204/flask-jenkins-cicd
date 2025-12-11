pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "pip3 install -r requirements.txt"
            }
        }

        stage('Unit Test') {
            steps {
                sh """
                    python3 -m py_compile app.py
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t local/myapp:${BUILD_NUMBER} ."
            }
        }

        stage('Deploy Container Locally') {
            steps {
                sh """
                    docker stop myapp || true
                    docker rm myapp || true
                    docker run -d -p 5000:5000 --name myapp local/myapp:${BUILD_NUMBER}
                """
            }
        }
    }
}
