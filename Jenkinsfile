pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'ajithdocgym@gmail.com'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the repository...'
                checkout scm
            }
        }

        stage('Run Script') {
            steps {
                echo 'Running hello.sh script...'
                sh 'chmod +x hello.sh'
                sh './hello.sh'
            }
        }
    }

    post {
        success {
            echo 'Build succeeded. Sending success email...'
            script {
                try {
                    emailext(
                        subject: "Build Successful: ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "Good news! The build succeeded.\n\nCheck console: ${BUILD_URL}",
                        to: "${EMAIL_RECIPIENT}"
                    )
                } catch (Exception e) {
                    echo "Email failed: ${e.message}"
                }
            }
        }

        failure {
            echo 'Build failed. Sending failure email...'
            script {
                try {
                    emailext(
                        subject: "Jenkins Job '${JOB_NAME} [${BUILD_NUMBER}]' Failed",
                        body: """<p>Oops!</p>
                                  <p>The Jenkins job <b>${JOB_NAME}</b> build #${BUILD_NUMBER} failed.</p>
                                  <p>Check console output <a href="${BUILD_URL}console">${BUILD_URL}console</a></p>""",
                        to: "${EMAIL_RECIPIENT}"
                    )
                } catch (Exception e) {
                    echo "Email failed: ${e.message}"
                }
            }
        }
    }
}
