pipeline {
    agent any
    environment {
        // Ensure GIT_PREVIOUS_COMMIT is set correctly
        GIT_PREVIOUS_COMMIT = sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('CS Helm Charts Dev') {
            when {
                expression {
                    return hasChanges('environments/dev/app-config')
                }
            }
            steps {
                echo 'Running stages for environments/dev/app-config...'
                // Add your pipeline steps here
                sh "ls -la"
            }
        }
        stage('Admin Helm Charts SIT') {
            when {
                expression {
                    return hasChanges('sit/global-settings.yaml')
              }
            }
            steps {
                echo 'Running stages for sit/global-settings.yaml...'
                // Add your pipeline steps here
                sh "pwd"
 
            }
        }
      
    }
}
def hasChanges(String path) {
    // Ensure GIT_COMMIT is available, otherwise default to the latest commit
    def commit = env.GIT_COMMIT ?: sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
    def previousCommit = env.GIT_PREVIOUS_COMMIT ?: sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
    // Get the list of changed files between the two commits
    def changes = sh(returnStdout: true, script: "git diff --name-only ${previousCommit} ${commit}").trim()
    // Check if any of the changed files match the provided path
    return changes.split('\n').any { it.startsWith(path) }
}
