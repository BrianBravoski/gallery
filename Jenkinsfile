pipeline {
    agent any
    environment {
        EMAIL_TEXT =
       """
         <h1>Build Notification</h1>
            <p>Build Status: ${currentBuild.currentResult}</p>
            <p>Project: ${env.JOB_NAME}</p>
            <p>Build Number: ${env.BUILD_NUMBER}</p>
            <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
            <p>Branch: ${env.GIT_BRANCH}</p>
            <p>Commit: ${env.GIT_COMMIT}</p>
            <p>Commit Message: ${env.GIT_COMMIT_MESSAGE}</p>
       """
        EMAIL_SUBJECT_SUCCESS = "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        EMAIL_SUBJECT_FAILURE = "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        EMAIL_RECIPIENTS = 'brianbravoski28@gmail.com'
        LIVE_SITE = 'https://gallery-bdr7.onrender.com'
    }
    tools {
        nodejs 'nodejs'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/BrianBravoski/gallery.git'
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
    post {
        success {
            slackSend(
                color: 'good',
                message: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER} - <${LIVE_SITE}|View Live Site>"
            )
            mail(
                to: EMAIL_RECIPIENTS,
                subject: EMAIL_SUBJECT_SUCCESS,
                body: EMAIL_TEXT,
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER} - <${LIVE_SITE}|View Live Site>"
            )
            mail(
            to: EMAIL_RECIPIENTS,
            subject: EMAIL_SUBJECT_FAILURE,
            body: EMAIL_TEXT,
        )
        }
    }
}
