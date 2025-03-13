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

        stage(' Dev') {
            when {
                expression {
                    return hasChanges('dev/')
                }
            }
            steps {
                echo 'Running stages for dev...'
                sh "ls -la"
            }
        }
        
        stage('SIT') {
            when {
                expression {
                    return hasChanges('sit/')
                }
            }
            steps {
                echo 'Running stages for SIT...'
                sh "ls -la"
            }
        }
     stage('UAT') {
            when {
                expression {
                    return hasChanges('uat/')
                }
            }
            steps {
                echo 'Running stages for UAT...'
                sh "ls -la"
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
