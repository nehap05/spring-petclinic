pipeline {
    agent any
    
    triggers {
        cron('H/10 * * * 1')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nehap05/spring-petclinic.git'
            }
        }

        stage('Build and Test') {
            parallel {
                stage('Build') {
                    steps {
                        script {
                            sh './mvnw clean package -DskipTests'
                        }
                    }
                }

                stage('Test') {
                    steps {
                        script {
                            sh './mvnw test'
                        }
                    }
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    sh './mvnw jacoco:report' 
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