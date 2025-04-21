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
            emailext (
                subject: "Jenkins Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' Succeeded",
                body: """<p>Good news!</p>
                         <p>The Jenkins job <b>${env.JOB_NAME}</b> build #${env.BUILD_NUMBER} was successful.</p>
                         <p>Check console output <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: "${env.EMAIL_RECIPIENT}"
            )
        }

        failure {
            echo 'Build failed. Sending failure email...'
            emailext (
                subject: "Jenkins Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed",
                body: """<p>Oops!</p>
                         <p>The Jenkins job <b>${env.JOB_NAME}</b> build #${env.BUILD_NUMBER} failed.</p>
                         <p>Check console output <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: "${env.EMAIL_RECIPIENT}"
            )
        }
    }
}
