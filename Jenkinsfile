pipeline {
    agent any
    
    stages {
        stage('Pegar o código no repositório') {
            steps {
                git 'https://github.com/Camiloverner/pipeline-hello_world.git'
            }
        }
        
        stage('Fazer o deploy na web') {
            steps {
                // Copiar os arquivos do repositório para o diretório do Apache
                sh 'cp -r * /var/www/html'
            }
        }
    }
}
