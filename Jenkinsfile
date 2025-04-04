pipeline {
  agent any

  parameters {
    booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply without asking?')
    choice(name: 'action', choices: ['apply', 'destroy'], description: 'Terraform action to perform')
  }

  environment {
    AWS_ACCESS_KEY_ID     = credentials('aws-access-key')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
    AWS_DEFAULT_REGION    = 'us-east-1'
  }

  stages {
    stage('Terraform Init') {
      steps {
        bat 'terraform init'
      }
    }

    stage('Terraform Plan') {
      steps {
        bat 'terraform plan -out=tfplan'
        bat 'terraform show -no-color tfplan > tfplan.txt'
      }
    }

    stage('Terraform Apply / Destroy') {
      steps {
        script {
          if (params.action == 'apply') {
            if (!params.autoApprove) {
              def plan = readFile('tfplan.txt')
              input message: "Do you want to apply this Terraform plan?",
                parameters: [
                  text(name: 'Plan', description: 'Review the plan before proceeding', defaultValue: plan)
                ]
            }
            bat 'terraform apply -input=false tfplan'
          } else if (params.action == 'destroy') {
            bat 'terraform destroy -auto-approve'
          } else {
            error "Invalid action selected. Please choose either 'apply' or 'destroy'."
          }
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'tfplan.txt', onlyIfSuccessful: true
    }
  }
}
