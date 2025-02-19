pipeline {
    agent any

    tools {
        maven 'MAVEN3'   
        jdk 'JAVA_HOME'     
    }

    triggers {
        cron('H/10 * * * 1')  // Runs every 10 minutes on Mondays
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
                            bat 'mvnw.cmd clean package -DskipTests'  
                        }
                    }
                }

                stage('Test') {
                    steps {
                        script {
                            bat 'mvnw.cmd test'  
                        }
                    }
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    bat 'mvnw.cmd jacoco:report'  
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}