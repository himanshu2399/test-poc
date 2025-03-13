pipeline {
    agent any
    triggers {
        githubPush()  // Automatically trigger on GitHub push event
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Check Changes in Specific Folder') {
            steps {
                script {
                    def changedFiles = sh(script: "git diff --name-only HEAD~1 HEAD", returnStdout: true).trim()
                    echo "Changed files: ${changedFiles}"

                    if (changedFiles.split('\n').any { it.startsWith('environments/dev/app-config/') }) {
                        echo "Relevant changes detected! Proceeding with the build."
                    } else {
                        echo "No relevant changes detected. Skipping build."
                        error("No relevant changes in monitored folder.")
                    }
                }
            }
        }
        stage('Run Build') {
            steps {
                echo 'Building the application...'
            }
        }
    }
}
