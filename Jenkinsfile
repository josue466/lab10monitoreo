pipeline {
    agent any

    environment {
        APP_NAME = "lab10monitoreo"
        VERSION = "1.0.${BUILD_NUMBER}"
        REPO_URL = "https://github.com/lesantivanez/lab10monitoreo.git"
        BRANCH = "main"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Clonando repositorio..."
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Instalando dependencias..."
                sh 'npm install'
            }
        }

        stage('Run Tests (Jest)') {
            steps {
                echo "Ejecutando pruebas..."
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Construyendo imagen Docker..."
                sh "docker build -t ${APP_NAME}:${VERSION} ."
            }
        }

        stage('Deploy') {
            steps {
                echo "Desplegando con Docker Compose..."
                sh 'docker compose down || true'
                sh 'docker compose up -d --build'
            }
        }

        stage('DORA Metrics (Simulado)') {
            steps {
                echo "📊 Métricas DORA"
                echo "Deployment Frequency: ${BUILD_NUMBER}"
                echo "Lead Time: Simulado"
                echo "MTTR: Simulado"
                echo "Change Failure Rate: Simulado"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline ejecutado correctamente - Version ${VERSION}"
        }
        failure {
            echo "❌ Error en pipeline"
        }
    }
}