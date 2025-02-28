pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://your-sonarqube-server:9000' // The name configured in Jenkins
        SONAR_SCANNER = tool 'Sonar' // SonarQube scanner installation in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repository.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Use the configured SonarQube server
                    sh "${SONAR_SCANNER}/bin/sonar-scanner \
                        -Dsonar.projectKey=sample2 \
                        -Dsonar.sources=src \
                        -Dsonar.host.url=${SONARQUBE_URL}  \
                        -Dsonar.login=sqp_500cea5e8c455799301381a0520d16dba10a9f73"
                }
            }
        }
    }
}
