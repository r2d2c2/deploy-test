pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk' // Java 환경 설정
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

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
                sh 'chmod +x ./mvnw' // 실행 권한 추가
                echo 'Building the Spring Boot application...'
                sh './mvnw clean package' // Maven 빌드
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
                sh 'nohup java -jar target/*.jar &' // Spring Boot 실행
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
