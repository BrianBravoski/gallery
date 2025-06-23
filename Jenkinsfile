pipeline {
    agent any
    environment {
        EMAIL_TEXT =
       """
         Build Notification
            Build Status: ${currentBuild.currentResult}
            Project: ${env.JOB_NAME}
            Build Number: ${env.BUILD_NUMBER}
            Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>
            Branch: ${env.GIT_BRANCH}
            Commit: ${env.GIT_COMMIT}
            Commit Message: ${env.GIT_COMMIT_MESSAGE}
       """
        EMAIL_SUBJECT_SUCCESS = "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        EMAIL_SUBJECT_FAILURE = "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        EMAIL_RECIPIENTS = "kemboidev@gmail.com"
        LIVE_SITE = "https://gallery-1-ir52.onrender.com"
        RENDER_DEPLOY_HOOK = credentials('render_hook')
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
        // stage('Build') {
        //     steps {
        //         sh 'npm build'
        //     }
        // }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                sh "curl -X POST ${RENDER_DEPLOY_HOOK}"
            }
        }
    }
    post {
        success {
            slackSend(
                color: 'good',
                message: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER} - <${LIVE_SITE}|View Live Site>"
            )
            // mail(
            //     to: EMAIL_RECIPIENTS,
            //     subject: EMAIL_SUBJECT_SUCCESS,
            //     body: EMAIL_TEXT,
            // )
        }
        failure {
            // slackSend(
            //     color: 'danger',
            //     message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER} - <${LIVE_SITE}|View Live Site>"
            // )
            mail(
            to: EMAIL_RECIPIENTS,
            subject: EMAIL_SUBJECT_FAILURE,
            body: EMAIL_TEXT,
        )
        }
    }
}
