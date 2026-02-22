pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'npm run build'
                sh 'npm test'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t cicd-node-app:${BUILD_NUMBER} .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f cicd-pipeline || true
                docker run -d -p 3000:3000 --name cicd-pipeline cicd-node-app:${BUILD_NUMBER}
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Application deployed successfully using Docker'
        }
        failure {
            echo 'pipeline failed'
        }
        
    }
}
