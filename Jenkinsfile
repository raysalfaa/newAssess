pipeline {
    agent any

    environment {
        GIT_URL = 'https://github.com/raysalfaa/newAssess.git'
        // BASE_BRANCH = ''
        // SOURCE_BRANCH = ''
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    def json = readJSON(text: env.CHANGE_ID ? env.GIT_COMMIT_MESSAGE : '{}')
                    
                    BASE_BRANCH = env.CHANGE_TARGET // Target branch (e.g., dev, prod, staging)
                    SOURCE_BRANCH = env.CHANGE_BRANCH // Source branch (PR source branch)
                    GIT_URL = env.GIT_URL // Clone URL of repo

                    echo "Base Branch (Target): ${BASE_BRANCH}"
                    echo "Source Branch: ${SOURCE_BRANCH}"
                    echo "Clone URL: ${GIT_URL}"
                    
                    // Skip build if the target branch is prod or staging
                    if (BASE_BRANCH == 'prod' || BASE_BRANCH == 'staging') {
                        echo "Skipping build for target branch: ${BASE_BRANCH}"
                        currentBuild.result = 'ABORTED'
                        return
                    }

                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building the application..."
                // Add your build commands here
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Add test commands here
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed!"
        }
    }
}
