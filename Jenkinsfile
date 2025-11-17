pipeline {
    agent any

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
                python -m venv venv
                call venv\\Scripts\\activate.bat
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                bat """
                call venv\\Scripts\\activate.bat
                pytest
                """
            }
        }

        stage('Deploy (Windows)') {
            steps {
                bat """
                call venv\\Scripts\\activate.bat
                echo Starting Flask app in background...
                start /B python app.py
                """
            }
        }

    }
}
