pipeline {
    agent {
        docker {
            image 'docker:dind'
            args '--privileged'
        }
    }
    stages {
        stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
    }
}