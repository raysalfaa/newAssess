pipeline {
    agent any
    
    stages {
        stage('Fetch Code') {
            steps {
                git <your-github-repository>
            }
        }
        stage('Code Analysis') {
            environment {
                scannerHome = tool 'Sonar'
            }
            steps {
                script {
                    withSonarQubeEnv('Sonar') {
                        sh "${sonar-scanner \
                              -Dsonar.projectKey=sample2 \
                              -Dsonar.sources=. \
                              -Dsonar.host.url=http:localhost:9000 \
                              -Dsonar.token=sqp_500cea5e8c455799301381a0520d16dba10a9f73"
                    }
                }
            }
        }
    }
}
