pipeline {
    agent any
    environment {
        CHANGED_FILES = "${env.CHANGED_FILES}" // Get file changes from webhook
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    // Fallback in case webhook doesn't set previous commit
                    env.GIT_PREVIOUS_COMMIT = env.GIT_PREVIOUS_COMMIT ?: sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
                }
            }
        }

        stage('Dev') {
            when {
                expression { hasChanges('dev') }
            }
            steps {
                echo 'Running stages for Dev...'
                sh "ls -la"
            }
        }

        stage('SIT') {
            when {
                expression { hasChanges('sit') }
            }
            steps {
                echo 'Running stages for SIT...'
                sh "ls -la"
            }
        }

        stage('UAT') {
            when {
                expression { hasChanges('uat') }
            }
            steps {
                echo 'Running stages for UAT...'
                sh "ls -la"
            }
        }
    }
}

def hasChanges(String path) {
    def changedFiles = env.CHANGED_FILES?.trim()

    if (changedFiles) {
        // Webhook provides file changes directly
        return changedFiles.tokenize(',').any { it.startsWith(path) }
    } else {
        // Fallback to git diff if webhook data is missing
        def commit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
        def previousCommit = env.GIT_PREVIOUS_COMMIT ?: sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
        def changes = sh(returnStdout: true, script: "git diff --name-only ${previousCommit} ${commit}").trim()
        return changes.tokenize('\n').any { it.startsWith(path) }
    }
}
