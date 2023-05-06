pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building code with Maven'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit tests with JUnit'
                echo 'Running integration tests with Selenium'
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code with SonarQube'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Scanning code with OWASP ZAP'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to AWS EC2 instance'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to AWS EC2 instance'
            }
        }
    }
    post {
        success {
            emailext (
                attachLog: true, 
                to: 'prajwalkantharaju@gmail.com',
                subject: 'Pipeline succeeded',
                body: 'The pipeline has succeeded. Please check the logs for details.',
                
            )
        }

        failure {
            emailext (
                subject: "Pipeline Failed",
                body: "The pipeline has failed. See attached logs.",
                to: "yashjangra617@gmail.com",
                attachLog: true,
            )
        }
    }
}
