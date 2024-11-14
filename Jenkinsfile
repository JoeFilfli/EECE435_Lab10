pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\activate && pip install -r requirements.txt'
            }
        }
        stage('Lint') {
            steps {
                bat 'venv\\Scripts\\activate && flake8 app.py'
            }
        }
        stage('Test') {
            steps {
                bat 'venv\\Scripts\\activate && pytest'
            }
        }
    }
}
