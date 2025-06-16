pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/moonwellSSD/aboba'
            }
        }
        
        stage('Build and Deploy') {
            steps {
                sh '''
                    sudo apt-get update && sudo apt-get install -y docker.io docker-compose
                    docker-compose down || true
                    docker-compose up -d --build
                    sleep 10
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    docker exec redis-master redis-cli PING | grep PONG || exit 1
                    echo "Redis Cluster работает!"
                '''
            }
        }
    }
    
    post {
        always {
            sh 'docker-compose down'
        }
    }
}
