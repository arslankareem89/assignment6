pipeline {

    agent {
        label 'jenkins-agent1'
    }

    stages {

        stage('Checkout') {
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
            }
        }

        stage('SonarQube') {
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
                sh 'cd assignment5 && docker build -t assignment5:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag assignment5:latest arslankareem89/assignment5:latest
                        docker push arslankareem89/assignment5:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker pull arslankareem89/assignment5:latest
                    docker stop assignment5 2>/dev/null || true
                    docker rm assignment5 2>/dev/null || true
                    docker run -d --name assignment5 --restart unless-stopped -p 80:80 arslankareem89/assignment5:latest
                '''
            }
        }
    }

    post {
        success {
            emailext(
                to: 'arslan.kareem@camp2.tkxel.com',
                subject: 'Assignment 06 - SUCCESS',
                body: 'Pipeline completed successfully.'
            )
        }

        failure {
            emailext(
                to: 'arslan.kareem@camp2.tkxel.com',
                subject: 'Assignment 06 - FAILED',
                body: 'Pipeline failed.'
            )
        }
    }
}
