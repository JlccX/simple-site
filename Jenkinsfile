pipeline {
    agent any

    parameters {
        string(name: 'COMMIT_ID', defaultValue: '', description: 'Enter the Git commit ID to checkout')
    }

    stages {
        stage('Input Commit ID or Use Latest') {
            steps {
                script {
                    timeout(time: 1, unit: 'MINUTES') {
                        input message: 'Enter the Git commit ID', parameters: [string(name: 'COMMIT_ID', defaultValue: '', description: 'Git Commit ID')]
                    }
                    if (!params.COMMIT_ID) {
                        echo "Timeout reached. Fetching the latest commit ID."
                        COMMIT_ID = sh(returnStdout: true, script: 'git ls-remote https://github.com/JlccX/simple-site.git HEAD | awk \'{print $1}\'').trim()
                        echo "Using latest commit ID: ${COMMIT_ID}"
                    }

                    if (params.COMMIT_ID) {
                        echo "The commit ID recovered is: ${params.COMMIT_ID}"
                    } else {
                      echo "The latest commit ID will be used by default!"
                    }
                }
            }
        }

        
        stage('Checkout Repository') {
            steps {
                script {
                    // Clone the repository and checkout the specified commit
                    checkout([$class: 'GitSCM',
                              branches: [[name: params.COMMIT_ID]],
                              userRemoteConfigs: [[url: 'https://github.com/JlccX/simple-site.git']]
                    ])
                    sh """
                        echo 'The current workspace folder path is:'
                        pwd
                        echo "The current folder content is:"
                        ls
                        echo "The current commid ID hash is:"
                        git rev-parse HEAD
                        echo "The commit ID comment is:"
                        git log -1 --pretty=%B
                    """
                }
            }
        }

        stage('Verify Commit') {
            steps {
                script {
                    echo "Checked out to commit: ${params.COMMIT_ID}"
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
