pipeline {
    agent any

    parameters {
        string(name: 'COMMIT_ID', defaultValue: '', description: 'Enter the Git commit ID to checkout')
    }

    stages {
        stage('Input Commit ID or Use Latest') {
            steps {
                script {
                    try {
                        timeout(time: 2, unit: 'MINUTES') {
                            input message: 'Enter the Git commit ID', parameters: [string(name: 'COMMIT_ID', defaultValue: '', description: 'Git Commit ID')]
                        }
                     } catch (e) {
                        if (e instanceof org.jenkinsci.plugins.workflow.steps.FlowInterruptedException) {
                            echo "Timeout reached. Getting the latest commit ID."
                            COMMIT_ID = sh(returnStdout: true, script: 'git ls-remote https://github.com/JlccX/simple-site.git HEAD | awk \'{print $1}\'').trim()
                            echo "Using latest commit ID: ${COMMIT_ID}"
                        } else {
                            throw e
                        }
                    }
                }
            }
        }

        stage('Checkout Repository') {
            steps {
                script {
                    // Clone the repository and checkout the specified commit
                    checkout([$class: 'GitSCM',
                              branches: [[name: COMMIT_ID]],
                              userRemoteConfigs: [[url: 'https://github.com/your-repo.git']]
                    ])
                }
            }
        }

        stage('Verify Commit') {
            steps {
                script {
                    echo "Checked out to commit: ${COMMIT_ID}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
    }
}
