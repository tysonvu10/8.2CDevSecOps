pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'email-notification',
                    url: 'https://github.com/tysonvu10/8.2CDevSecOps.git',
                    credentialsId: 'github-credentials'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins Build #${env.BUILD_NUMBER} - Run Tests: ${currentBuild.currentResult}",
                        body: """
                            <h3>Jenkins Pipeline Notification</h3>
                            <p><b>Job:</b> ${env.JOB_NAME}</p>
                            <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                            <p><b>Stage:</b> Run Tests</p>
                            <p><b>Status:</b> ${currentBuild.currentResult}</p>
                            <p>Please find the build log attached.</p>
                        """,
                        to: 'your_email@gmail.com',
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins Build #${env.BUILD_NUMBER} - NPM Audit: ${currentBuild.currentResult}",
                        body: """
                            <h3>Jenkins Pipeline Notification</h3>
                            <p><b>Job:</b> ${env.JOB_NAME}</p>
                            <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                            <p><b>Stage:</b> NPM Audit (Security Scan)</p>
                            <p><b>Status:</b> ${currentBuild.currentResult}</p>
                            <p>The security scan log is attached.</p>
                        """,
                        to: 'your_email@gmail.com',
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }

    }
}