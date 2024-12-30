env.CommitID = ""

pipeline {
    agent any

    stages {
        stage('Input Commit ID or Use Latest') {
            steps {
                script {
                    try {
                        timeout(time: 2, unit: 'MINUTES') {
                            env.CommitID = input message: 'Enter the Git commit ID', parameters: [string(defaultValue: '', description: 'Please add the Git Commit ID required or leave it empty to get the latest one by default.', name: 'userInput')]
                        }
                     } catch (e) {
                        if (e instanceof org.jenkinsci.plugins.workflow.steps.FlowInterruptedException) {
                            echo "Timeout reached. Getting the latest commit ID."
                            sh """
                            echo "The latest commit ID is:"
                            git ls-remote https://github.com/JlccX/simple-site.git refs/heads/"${env.BRANCH_NAME}"
                            """
                            env.CommitID = sh(returnStdout: true, script: 'git ls-remote https://github.com/JlccX/simple-site.git refs/heads/"${env.BRANCH_NAME}" | awk \'{print $1}\'').trim()
                            echo "Using latest commit ID: ${env.CommitID}"
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
                    sh "echo '++++++++++++++++++++++ Checkout the repository +++++++++++++++++++++++++++++++++++++++++++++'"
                    checkout([$class: 'GitSCM',
                              branches: [[name: env.CommitID]],
                              userRemoteConfigs: [[url: 'https://github.com/JlccX/simple-site.git']]
                    ])
                }
            }
        }

        stage('Verify Commit') {
            steps {
                script {
                    echo "Checked out to commit: ${env.CommitID} "
                }
            }
        }


        stage('Playwright') {
            steps {
                script {
                    docker.image('mcr.microsoft.com/playwright:latest').inside {
                        sh 'npx playwright --version'
                    }
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
