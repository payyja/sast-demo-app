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
                    pip install --upgrade pip
                    pip install bandit
                '''
            }
        }

        stage('SAST Analysis') {
            steps {
                script {
                    // Jalankan Bandit dan abaikan exit code jika ada findings (Bandit mengembalikan kode != 0)
                    def status = sh(script: """
                        . ${VENV_DIR}/bin/activate
                        bandit -f xml -o bandit-output.xml -r . || true
                    """, returnStatus: true)

                    echo "Bandit finished with status ${status}"
                }
                // Rekam hasil analisis menggunakan plugin Warnings Next Generation
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
