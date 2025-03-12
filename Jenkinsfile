pipeline {
    agent any
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Check Changes') {
            steps {
                script {
                    def changedFiles = sh(script: "git diff --name-only HEAD~1 HEAD", returnStdout: true).trim()
                    echo "Changed files: ${changedFiles}"

                    // Only proceed if changes are inside 'environments/dev/app-config/'
                    if (!changedFiles.split("\n").find { it.startsWith("environments/dev/app-config/") }) {
                        echo "No relevant changes detected in environments/dev/app-config/. Skipping build."
                        error("No relevant changes detected")
                    } else {
                        echo "Relevant changes detected! Proceeding with the build."
                    }
                }
            }
        }

        stage('Run Build') {
            steps {
                echo "Building the application..."
                // Your build commands here
            }
        }
    }
}
