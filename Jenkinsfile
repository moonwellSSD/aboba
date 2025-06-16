pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                url: 'https://github.com/moonwellSSD/aboba'
            }
        }
        
        stage('Setup Environment') {
            steps {
                script {
                    def dockerInstalled = sh(script: 'command -v docker', returnStatus: true) == 0
                    def composeInstalled = sh(script: 'command -v docker-compose', returnStatus: true) == 0
                    
                    if (!dockerInstalled || !composeInstalled) {
                        error('Docker и docker-compose должны быть предустановлены на агенте Jenkins!')
                    }
                }
            }
        }
        
        stage('Deploy Cluster') {
            steps {
                sh '''
                    docker-compose down || true
                    docker-compose up -d --build
                    sleep 10
                '''
            }
        }
        
        stage('Verify') {
            steps {
                sh '''
                    docker ps
                    docker exec redis-master redis-cli ping | grep PONG || exit 1
                    echo "Redis Cluster работает!"
                '''
            }
        }
    }
    
    post {
        always {
            sh 'docker-compose down || true'
        }
    }
}
