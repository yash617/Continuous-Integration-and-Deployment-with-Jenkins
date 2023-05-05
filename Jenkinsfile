pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                sh './run_tests.sh'
            }
        }

        stage('Code Analysis') {
            steps {
                withMaven(maven: 'maven_3_8_1') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

         stage('Security Scan') {
            steps {
                // Use OWASP ZAP to perform a security scan
                sh 'zap.sh -cmd -target http://localhost:8080 -report report.html'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh './deploy.sh staging'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                sh './run_integration_tests.sh staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh './deploy.sh production'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "Pipeline Successful",
                body: "The pipeline has run successfully. See attached logs.",
                to: "yashjangra617@gmail.com",
                attachmentsPattern: '**/*.log'
            )
        }

        failure {
            emailext (
                subject: "Pipeline Failed",
                body: "The pipeline has failed. See attached logs.",
                to: "yashjangra617@gmail.com",
                attachmentsPattern: '**/*.log'
            )
        }
    }
}
