pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sjo9/COMP367-spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh './mvnw clean package' 
                }
            }
        }

        stage('Test and Code Coverage') {
            steps {
                script {
                    sh './mvnw test' 
                }
            }
            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/target/classes', sourcePattern: '**/src/main/java'
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
