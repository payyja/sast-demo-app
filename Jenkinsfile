pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'git@github.com:payyja/sast-demo-app.git', 
                     branch: 'master', 
                     credentialsId: 'jenkins-ssh-key'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install bandit'
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '. venv/bin/activate && bandit -f xml -o bandit-output.xml -r . || true'
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
