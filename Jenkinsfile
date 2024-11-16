pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm // Lấy mã nguồn từ repository
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker-compose down' // Dừng các container cũ
                    sh 'docker-compose build' // Build các images từ Dockerfile
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    echo 'Starting Docker Compose with specified file'
                    sh 'docker-compose -f compose.yaml up -d'
                }
            }
}


        stage('Verify Deployment') {
            steps {
                script {
                    sh 'docker ps' // Kiểm tra các container đang chạy
                }
            }
        }
    }
}
