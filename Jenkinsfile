pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    env.GIT_PREVIOUS_COMMIT = sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
                }
            }
        }
        stage('Check Folder Changes') {
            steps {
                script {
                    if (!hasChanges('environments/dev/app-config') && !hasChanges('sit/global-settings.yaml')) {
                        echo "No relevant changes detected. Skipping pipeline."
                        currentBuild.result = 'SUCCESS'
                        error("Stopping build as no relevant changes found")
                    }
                }
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
                sh "pwd"
            }
        }
    }
}

def hasChanges(String path) {
    def commit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
    def previousCommit = env.GIT_PREVIOUS_COMMIT ?: sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
    def changes = sh(returnStdout: true, script: "git diff --name-only ${previousCommit} ${commit}").trim()
    return changes.tokenize('\n').any { it.startsWith(path) }
}
