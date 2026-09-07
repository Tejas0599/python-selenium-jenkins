pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Set Up Environment & Dependencies') {
            steps {
                sh '''
                    echo "[1/3] Creating virtual environment..."
                    rm -rf venv
                    python3 -m venv venv

                    echo "[2/3] Upgrading pip..."
                    ./venv/bin/python -m pip install --upgrade pip

                    echo "[3/3] Installing testing packages..."
                    ./venv/bin/python -m pip install pytest selenium webdriver-manager
                '''
            }
        }

        stage('Execute Selenium Tests') {
            steps {
                sh '''
                    mkdir -p reports

                    echo "Running Pytest Suite..."
                    ./venv/bin/python -m pytest tests/ --junitxml=reports/junit-report.xml
                '''
            }
        }
    }

    post {
        always {
            junit testResults: 'reports/junit-report.xml',
                 allowEmptyResults: true
        }
    }
}