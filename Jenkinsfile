pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
        HOST = "0.0.0.0"
        PORT = "5000"
        APP_MODULE = "app:app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jihanebouramtane/Jenkins-CI-CD.git'
            }
        }

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

        stage('Run Tests') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate.bat
                pytest test_app.py -v
                """
            }
        }

        stage('Deploy') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate.bat
                gunicorn --bind %HOST%:%PORT%  %APP_MODULE% --pid gunicorn.pid --log-level debug
                """
            }
        }
    }
}
