pipeline {
    agent any
    
    triggers {
        cron('H/10 * * * 1') // 매주 월요일 10분 간격
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sjo9/COMP367-spring-petclinic.git'
            }
        }

        stage('Build and Test') {
            parallel {
                stage('Build') {
                    steps {
                        script {
                            sh './mvnw clean package -DskipTests'  // 테스트를 건너뛰고 빌드만 실행
                        }
                    }
                }

                stage('Test') {
                    steps {
                        script {
                            sh './mvnw test'  // 테스트만 실행
                        }
                    }
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    sh './mvnw jacoco:report'  // Jacoco 리포트 생성
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}
