pipeline {
    agent any

   

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning Git Repository...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Granting execute permission to mvnw...'
                sh 'chmod +x ./mvnw' // Maven Wrapper 실행 권한 추가
                echo 'Building the Spring Boot application...'
                sh './mvnw clean package' // Maven 빌드 실행
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh './mvnw test' // 테스트 실행
            }
        }
        stage('Run') {
            steps {
                echo 'Starting the Spring Boot application...'
                sh 'nohup java -jar target/*.jar &' // Spring Boot 애플리케이션 실행
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
