pipeline {
    agent any

    parameters {
        string(name: 'COMMIT_ID', defaultValue: '', description: 'Enter the Git commit ID to checkout')
    }

    stages {
        stage('Checkout Repository') {
            steps {
                script {
                    if (params.COMMIT_ID) {
                        echo "The commit ID recovered is: ${params.COMMIT_ID}"
                    } else {
                      echo "The latest commit ID will be used by default!"
                    }
                    // Clone the repository and checkout the specified commit
                    checkout([$class: 'GitSCM',
                              branches: [[name: params.COMMIT_ID]],
                              userRemoteConfigs: [[url: 'https://github.com/JlccX/simple-site.git']]
                    ])
                    echo "The current folder path is:"
                    sh "pwd"
                    sh "ls"
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
