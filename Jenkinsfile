pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'java11'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/PEKETIJAGADEESH/charity.git'
            }
        }

        stage('Build WAR using Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker rm -f charity-container || true
                docker rmi charity-app || true
                docker build -t charity-app .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d -p 8085:8080 --name charity-container charity-app
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Application deployed successfully using Jenkins Pipeline!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
    }
}
