pipeline {
    agent any
     environment {
        SONAR_HOST_URL = 'http://192.168.1.40:9000' // Thay thế bằng URL của SonarQube của bạn
           SONAR_AUTH_TOKEN = credentials('micro-sonar-token') 
    }
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm // Lấy mã nguồn từ repository
            }
        }

     stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner' // Đảm bảo tên đúng với tên công cụ đã cài đặt
                    withSonarQubeEnv('sonarqube') { // Tên server SonarQube phải giống với cấu hình trong Jenkins
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=micro \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_AUTH_TOKEN
                        """
                    }
                }
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
                    sh 'docker-compose -f docker-compose.yml up -d' // Khởi động các container mới
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
