```groovy
pipeline {

    agent {
        label 'jenkins-agent1'
    }

    stages {

        stage('Multi-SCM Checkout') {
            steps {

                dir('assignment5') {
                    git branch: 'master',
                        credentialsId: 'github-credentials',
                        url: 'https://github.com/arslankareem89/assignment5.git'
                }

                dir('wink_dashboard') {
                    git branch: 'develop',
                        credentialsId: 'github-credentials',
                        url: 'https://github.com/arslankareem89/wink_dashboard.git'
                }

                sh '''
                    echo "===== ASSIGNMENT 5 ====="
                    ls -la assignment5

                    echo "===== WINK DASHBOARD ====="
                    ls -la wink_dashboard
                '''
            }
        }

        stage('SonarQube - Assignment 5') {
            steps {
                dir('assignment5/react-app') {
                    script {
                        def scannerHome = tool 'sonar-scanner'

                        withSonarQubeEnv('sonarqube') {
                            sh """
                                ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=assignment5 \
                                -Dsonar.projectName=assignment5 \
                                -Dsonar.sources=.
                            """
                        }
                    }
                }
            }
        }

        stage('SonarQube - Wink Dashboard') {
            steps {
                dir('wink_dashboard') {
                    script {
                        def scannerHome = tool 'sonar-scanner'

                        withSonarQubeEnv('sonarqube') {
                            sh """
                                ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=wink-dashboard \
                                -Dsonar.projectName=wink-dashboard \
                                -Dsonar.sources=lib
                            """
                        }
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    echo "===== BUILD ASSIGNMENT 5 IMAGE ====="
                    cd assignment5
                    docker build -t assignment5:latest .

                    echo "===== BUILD WINK DASHBOARD IMAGE ====="
                    cd ../wink_dashboard
                    docker build -t wink-dashboard:latest .

                    echo "===== DOCKER IMAGES ====="
                    docker images
                '''
            }
        }
    }
}
```
