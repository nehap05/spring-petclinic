pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1')  // Runs every 10 minutes on Mondays
    }

    tools {
        maven 'MAVEN3'   
        jdk 'JAVA_HOME'      
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/nehap05/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:prepare-agent test jacoco:report'
            }
            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec', 
                           classPattern: '**/target/classes', 
                           sourcePattern: '**/src/main/java', 
                           inclusionPattern: '**/*.class', 
                           exclusionPattern: ''
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}