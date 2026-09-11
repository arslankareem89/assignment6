pipeline {
    agent {
        label 'jenkins-agent1'
    }

    stages {
        stage('Test Agent') {
            steps {
                sh 'hostname'
                sh 'whoami'
                sh 'pwd'
            }
        }
    }
}