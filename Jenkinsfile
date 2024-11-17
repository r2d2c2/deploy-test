pipeline {
    agent any

    tools {
        maven "M3" // Jenkins에 설정된 Maven 버전 이름
    }

    stages {
        stage('Checkout') {
            steps {
                // GitHub에서 소스 코드를 가져옵니다
                git url: 'https://github.com/lleellee0/deploy-test', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                // Maven을 사용해 프로젝트를 빌드합니다
                sh 'mvn clean package'
            }
        }
        
        stage('Run Application') {
            steps {
                script {
                    // Maven 빌드로 생성된 JAR 파일 경로
                    def jarFile = 'target/shortenurlservice-0.0.1-SNAPSHOT.jar'

                    // 백그라운드에서 애플리케이션 실행 (포트를 8081로 설정)
                    def logFile = 'app.log'
                    sh """
                        nohup setsid java -Dserver.port=8081 -jar $jarFile > $logFile 2>&1 &
                    """
                    
                    // 로그 파일을 확인하여 애플리케이션 시작 여부를 확인
                    sleep 10 // 애플리케이션이 시작되도록 대기
                    def checkLogCommand = "grep -q 'Started ShortenurlserviceApplication in' $logFile"
                    int result = sh script: checkLogCommand, returnStatus: true
                    
                    if (result == 0) {
                        echo 'Application started successfully on port 8081.'
                    } else {
                        error 'Application failed to start. Check the logs for details.'
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment completed successfully.'
        }
        failure {
            echo 'Deployment encountered an error.'
        }
    }
}
