pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/payyja/sast-demo-app'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install bandit
                '''
            }
        }

        stage('SAST Analysis') {
            steps {
                sh '''
                    . venv/bin/activate
                    bandit -f xml -o bandit-output.xml -r .
                '''
                echo 'Bandit finished with status 0'
            }
        }

        stage('Archive Bandit Report') {
            steps {
                archiveArtifacts artifacts: 'bandit-output.xml', fingerprint: true
            }
        }
    }
}
