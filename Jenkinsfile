pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/payyja/sast-demo-app.git', branch: 'master'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install bandit
                '''
            }
        }

        stage('SAST Analysis') {
            steps {
                sh '''
                    . ${VENV_DIR}/bin/activate
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
