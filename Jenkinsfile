pipeline {

    agent any

    options {
        skipDefaultCheckout(true)
    }

    environment {
        APP_NAME = "lab10monitoreo"
        VERSION = "1.0.${BUILD_NUMBER}"
        REPO_URL = "https://github.com/lesantivanez/lab10monitoreo.git"
        BRANCH = "main"
    }

    stages {

        // 🔹 Limpieza del workspace
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        // 🔹 Checkout simple y confiable
        stage('Checkout') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        // 🔹 Verificar que el repo se descargó correctamente
        stage('Verify Checkout') {
            steps {
                sh '''
                echo "=== WORKSPACE DESPUÉS DEL CHECKOUT ==="
                ls -R
                '''
            }
        }

        // 🔹 Instalar dependencias y correr tests dentro de Docker Node
        stage('Install & Test') {
            steps {
                sh '''
                docker run --rm \
                  -v $WORKSPACE:/app \
                  -w /app \
                  node:18 \
                  sh -c "
                    echo '📂 Contenido de /app:' && ls -la && \
                    if [ ! -f package.json ]; then
                        echo '❌ package.json no encontrado, abortando...' && exit 1
                    fi && \
                    npm install && npm test
                  "
                '''
            }
        }

        // 🔹 Build de la imagen Docker con versionado automático
        stage('Build Image') {
            steps {
                sh "docker build -t ${APP_NAME}:${VERSION} ."
            }
        }

        // 🔹 Deploy usando Docker Compose
        stage('Deploy') {
            steps {
                sh 'docker compose down || true'
                sh "APP_VERSION=${VERSION} docker compose up -d --build"
            }
        }

        // 🔹 Health Check para asegurar que la app está corriendo
        stage('Health Check') {
            steps {
                sh '''
                sleep 5
                curl -f http://localhost:3000 || exit 1
                '''
            }
        }
    }
}