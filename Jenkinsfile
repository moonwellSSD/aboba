pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sasha-VAV/redis-cluster_fork_for_fix.git'
            }
        }
        
        stage('Build and Deploy') {
            steps {
                sh '''
                    # Устанавливаем зависимости (если нужно)
                    sudo apt-get update && sudo apt-get install -y docker.io docker-compose
                    
                    # Запускаем Redis Cluster через docker-compose
                    docker-compose down || true  # Останавливаем старые контейнеры
                    docker-compose up -d --build
                    
                    # Ждём запуска
                    sleep 10
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    # Проверяем, что Redis отвечает
                    docker exec redis-master redis-cli PING | grep PONG || exit 1
                    echo "Redis Cluster работает!"
                '''
            }
        }
    }
    
    post {
        always {
            sh 'docker-compose down'  # Очистка после завершения
        }
    }
}
