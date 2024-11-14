pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHONPATH = "${env.WORKSPACE}" // Sets the current workspace as PYTHONPATH
    }
    stages {
        stage('Setup') {
            steps {
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\activate && pip install -r requirements.txt'
                bat 'venv\\Scripts\\activate && pip install coverage bandit' // Ensure coverage.py and bandit are installed
            }
        }
        stage('Lint') {
            steps {
                bat 'venv\\Scripts\\activate && flake8 app.py'
            }
        }
        stage('Test') {
            steps {
                bat 'venv\\Scripts\\activate && coverage run -m pytest'
            }
        }
        stage('Coverage') {
            steps {
                bat 'venv\\Scripts\\activate && coverage report'
                bat 'venv\\Scripts\\activate && coverage html' // Generate an HTML report
                archiveArtifacts artifacts: 'htmlcov/**', allowEmptyArchive: true // Archive coverage report
            }
        }
        stage('Security Scan') {
    steps {
        script {
            def banditStatus = bat(returnStatus: true, script: 'venv\\Scripts\\activate && bandit -r . -o bandit_report.txt --format txt')
            if (banditStatus != 0) {
                echo "Bandit found potential security issues. Check the report: bandit_report.txt"
            }
        }
        archiveArtifacts artifacts: 'bandit_report.txt', allowEmptyArchive: true
    }
}


        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    // Add deployment logic here, e.g., copy files to a server or trigger a deploy script
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
