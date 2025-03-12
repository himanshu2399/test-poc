pipeline {
    agent any
    triggers {
        githubPush()
    }
    environment {
        TARGET_FOLDER = "environments/dev/app-config/**"
    }
    stages {
        stage('Check Changes') {
            steps {
                script {
                    def changedFiles = sh(script: "git diff --name-only HEAD~1 HEAD", returnStdout: true).trim().split("\n")

                    if (!changedFiles.any { it.startsWith(env.TARGET_FOLDER) }) {
                        echo "No relevant changes detected. Skipping build."
                        currentBuild.result = 'ABORTED'
                        error("No relevant changes detected")
                    }
                }
            }
        }
        stage('Run Build') {
            steps {
                echo "Changes detected in ${env.TARGET_FOLDER}. Running build..."
                // Add your build/deploy steps here
            }
        }
    }
}
