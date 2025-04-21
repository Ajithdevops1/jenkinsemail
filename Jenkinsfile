pipeline {
    agent any

    environment {
        // Set email recipient
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
}

}
