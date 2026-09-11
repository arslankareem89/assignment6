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

        stage('SonarQube Analysis') {
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
    }
}