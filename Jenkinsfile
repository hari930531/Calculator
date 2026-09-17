pipeline {
    agent any

    environment {
        APP_NAME = "calculator-app"
        PORT = "8085"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo "Testing core frontend files exist..."
                // For Windows Jenkins agent:
                bat '''
                    if not exist index.html exit /b 1
                    if not exist style.css exit /b 1
                    if not exist script.js exit /b 1
                    echo All required files are present.
                '''
                /* If running on a Linux Jenkins agent, replace the 'bat' block above with:
                sh '''
                    test -f index.html || exit 1
                    test -f style.css || exit 1
                    test -f script.js || exit 1
                    echo "All required files are present."
                '''
                */
            }
        }

       stage('Build Docker Image') {
            steps {
               bat '''"C:\\Users\\ADMIN\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t calculator-app:latest .'''
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Docker Container..."
                bat '''
                    "C:\\Users\\ADMIN\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" stop %APP_NAME% 2>nul || ver >nul
                    "C:\\Users\\ADMIN\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" rm %APP_NAME% 2>nul || ver >nul
                    "C:\\Users\\ADMIN\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" run -d -p %PORT%:80 --name %APP_NAME% %APP_NAME%:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo "Verifying deployment..."
                bat '''
                    curl -I http://localhost:%PORT%
                '''
            }
        }
    }

    post {
        success {
            echo "Calculator deployed successfully! Open http://localhost:8085"
        }
        failure {
            echo "Pipeline failed. Check the console output logs."
        }
    }
}
