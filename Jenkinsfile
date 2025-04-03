pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
  }

  stages {
    stage('Terraform Init') {
      steps {
        dir('jenkins-aws-infra') {
          withCredentials([
            string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
            string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
          ]) {
            sh 'terraform init'
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
            sh '''
              terraform apply -auto-approve \
                -var "aws_region=${AWS_REGION}" \
                -var "ami_id=ami-05b10e08d247fb927" \
                -var "key_name=Aaa" \
                -var "instance_type=t2.micro" \
                -var "instance_name=Test2-EC2"
            '''
          }
        }
      }
    }
  }
}
