pipeline {
  agent any
  stages {
    stage('Terraform Init') {
      steps {
        dir('jenkins-aws-infra') {
          withCredentials([
            string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
            string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
          ]) {
            bat 'terraform init'
          }
        }
      }
    }

    stage('Terraform plan') {
      steps {
        dir('jenkins-aws-infra') {
          withCredentials([
            string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
            string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
          ]) {
            bat 'terraform plan -auto-approve'
          }
        }
      }
    }
    
    stage('Terraform Apply') {
      steps {
        dir('jenkins-aws-infra') {
          withCredentials([
            string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
            string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
          ]) {
            bat 'terraform apply -auto-approve'
          }
        }
      }
    }
  }
}
