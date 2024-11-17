pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk' // Java 환경 설정 (필요시 수정)
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning Git Repository...'
                checkout scm // 기본적으로 Git 소스 가져오기
            }
        }
        stage('Build') {
            steps {
                echo 'Building the Spring Boot application...'
                sh './mvnw clean package' // Maven Wrapper 사용
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
