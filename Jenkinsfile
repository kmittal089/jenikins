
pipeline {
    agent any

    environment {
        REPORT_DIR = 'reports'
        REPORT_FILE = 'report.html'
        RECIPIENTS = '9012521291khajan@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DeveshKapil/Devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv myenv
                    . myenv/bin/activate
                    pip install -r requirements.txt || true
                    pip install selenium pytest pytest-html
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . myenv/bin/activate
                    pytest --html=${REPORT_DIR}/${REPORT_FILE}
                '''
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: "${REPORT_DIR}",
                    reportFiles: "${REPORT_FILE}",
                    reportName: "Python Selenium Test Report"
                ])
            }
        }
    }

    post {
        always {
            emailext (
                to: "${RECIPIENTS}",
                subject: "Python Selenium Test Execution Report: ${currentBuild.fullDisplayName}",
                body: """
                    The Selenium tests have finished. You can view the detailed test report at:
                    ${BUILD_URL}htmlreports/${REPORT_FILE}
                    
                    For full build logs, see the attached log.
                """,
                attachLog: true
            )
        }
    }
}
