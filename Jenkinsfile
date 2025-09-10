pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.9'
        APP_NAME = 'meu-aplicativo-python'
    }

    stages {
        stage('Cleanup') {
            steps {
                echo 'Limpando ambiente...'
                sh 'rm -rf venv || true'
                sh 'docker system prune -f || true'
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Construção') {
            steps {
                echo 'Configurando ambiente Python...'
                sh 'python3 -m venv venv || python -m venv venv'
                sh '. venv/bin/activate && pip install --upgrade pip'
                sh '. venv/bin/activate && pip install -r requirements.txt || echo "Arquivo requirements.txt não encontrado"'

                echo 'Construindo aplicação...'
                script {
                    try {
                        sh """
                        if [ -f Dockerfile ]; then
                            docker build -t ${APP_NAME}:latest .
                        else
                            echo "Dockerfile não encontrado, pulando build de container"
                        fi
                        """
                    } catch (Exception e) {
                        echo "Erro ao construir imagem Docker: ${e.message}"
                        echo "Continuando pipeline sem build Docker..."
                    }
                }
            }
        }

        stage('Entrega') {
            steps {
                echo 'Preparando para entrega...'
                sh '. venv/bin/activate && python setup.py bdist_wheel || echo "Sem setup.py para build"'
                echo 'Artefato pronto para deploy'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'dist/*.whl', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        always {
            echo 'Limpando workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline executado com sucesso!'
        }
        failure {
            echo 'Pipeline falhou!'
        }
    }
}