pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/moonwellSSD/redis-cluster_Jenkinsfile'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down -v || true'
                sh 'docker-compose up -d'
            }
        }

        stage('Verify') {
            steps {
                sh 'docker ps | grep redis'
            }
        }
    }
}
