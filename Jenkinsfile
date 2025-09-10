pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
        stage('Checkout') {
            steps {
                git 'https://seu-repositorio.git'
            }
        }
        stage('Construção') {
            steps {
                sh 'docker build -t minha-app .'
            }
        }
        stage('Entrega') {
            steps {
                sh 'docker run -d --name minha-app minha-app'
            }
        }
    }
}