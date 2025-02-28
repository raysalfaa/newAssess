pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://your-sonarqube-server:9000'
        SONARQUBE_TOKEN = credentials('sonar') // Store token in Jenkins Credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    def BASE_BRANCH = env.CHANGE_TARGET
                    def SOURCE_BRANCH = env.CHANGE_BRANCH
                    def GIT_URL = env.GIT_URL
                    
                    echo "Base Branch (Target): ${BASE_BRANCH}"
                    echo "Source Branch: ${SOURCE_BRANCH}"
                    echo "Clone URL: ${GIT_URL}"

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
                sh './gradlew build'  // Change this based on your build tool (Maven, Gradle, etc.)
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {  // Use configured SonarQube server name
                    sh '''
                    ./gradlew sonar -Dsonar.projectKey=your_project_key \
                                   -Dsonar.host.url=$SONARQUBE_URL \
                                   -Dsonar.login=$SONARQUBE_TOKEN
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 2, unit: 'MINUTES') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline failed due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed!"
        }
    }
}
