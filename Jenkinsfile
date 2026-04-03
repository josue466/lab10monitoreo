pipeline {
    agent any

    environment {
        VERSION = "1.0.${BUILD_NUMBER}"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/tu-repo/devops-lab.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t devops-lab:${VERSION} ."
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose down'
                sh 'docker compose up -d'
            }
        }

        stage('DORA Metrics') {
            steps {
                echo "Deployment Frequency: ${BUILD_NUMBER}"
                echo "Lead Time: simulated"
            }
        }
    }
}