pipeline {
    agent any
    environment {
        CHANGED_FILES = sh(returnStdout: true, script: "git diff --name-only HEAD~1").trim()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Change') {
            steps {
                script {
                    def changedFiles = CHANGED_FILES.split('\n')

                    // Check if only 'uat/' files changed
                    if (changedFiles.every { it.startsWith('uat/') }) {
                        echo "Changes detected ONLY in 'uat' folder. Skipping build."
                        currentBuild.result = 'SUCCESS'
                        error("Skipping build as only 'uat' folder changed.")
                    } else {
                        echo "Relevant changes detected in: ${changedFiles}"
                    }
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
                echo 'Running stages for SIT...
                sh "ls -la"
            }
        }
    }
}

def hasChanges(String path) {
    def changedFiles = env.CHANGED_FILES ?: sh(returnStdout: true, script: "git diff --name-only HEAD~1").trim()
    return changedFiles.split('\n').any { it.startsWith(path) }
}
