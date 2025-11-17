pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
        HOST = "127.0.0.1"
        PORT = "5000"
    }

    stages {

        /* --- 1) CHECKOUT AUTOMATIQUE PAR JENKINS --- */
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jihanebouramtane/Jenkins-CI-CD.git'
            }
        }

        /* --- 2) CREA ENV PYTHON --- */
        stage('Setup Virtual Environment') {
            steps {
                bat """
                python -m venv %VENV_DIR%
                call %VENV_DIR%\\Scripts\\activate.bat
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        /* --- 3) TESTS UNITAIRES --- */
        stage('Run Tests') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate.bat
                pytest -v
                """
            }
        }

        /* --- 4) DEPLOIEMENT SUR WINDOWS AVEC FLASK --- */
        stage('Deploy (Windows)') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate.bat
                echo Starting Flask server...
                start /B python app.py
                """
            }
        }
    }

    /* --- NOTIFICATIONS EMAIL --- */
    post {
        success {
            emailext(
                to: "EMAIL_TO",
                subject: "SUCCESS: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The job ran successfully. Check details at ${env.BUILD_URL}"
            )
        }
        failure {
            emailext(
                to: "EMAIL_TO",
                subject: "FAILURE: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The job failed. See logs at ${env.BUILD_URL}"
            )
        }
    }
}
