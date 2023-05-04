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
        sh 'mvn test'
      }
    }

    stage('Code Analysis') {
      steps {
        withMaven(maven: 'Maven 3.8.1', jdk: 'OpenJDK 11') {
          sh 'mvn checkstyle:checkstyle'
        }
      }
    }

    stage('Security Scan') {
      steps {
        withMaven(maven: 'Maven 3.8.1', jdk: 'OpenJDK 11') {
          sh 'mvn dependency-check:check'
        }
      }
    }

    stage('Deploy to Staging') {
      steps {
        sh 'scp -i /path/to/keypair.pem target/my-app.jar ec2-user@staging:/home/ec2-user'
      }
    }

    stage('Integration Tests on Staging') {
      steps {
        sh 'ssh -i /path/to/keypair.pem ec2-user@staging "java -jar my-app.jar"'
      }
    }

    stage('Deploy to Production') {
      steps {
        sh 'scp -i /path/to/keypair.pem target/my-app.jar ec2-user@production:/home/ec2-user'
      }
    }
  }

  post {
    success {
      emailext(
        to: 'yashjangra617@gmail.com',
        subject: 'Pipeline succeeded',
        body: 'The pipeline has succeeded. Please check the logs for details.',
        attachmentsPattern: 'logs/*'
      )
    }

    failure {
      emailext(
        to: 'yashjangra617@gmail.com',
        subject: 'Pipeline failed',
        body: 'The pipeline has failed. Please check the logs for details.',
        attachmentsPattern: 'logs/*'
      )
    }
  }
}
