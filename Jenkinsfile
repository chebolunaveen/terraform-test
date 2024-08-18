properties([
    parameters([
        string(
            defaultValue: 'dev',
            name: 'Environment'
        ),
        choice(
            choices: ['plan', 'apply', 'destroy'], 
            name: 'Terraform_Action'
        )])
])
pipeline {
    agent any
    stages {
        stage('Preparing') {
            steps {
                sh 'echo Preparing'
            }
        }
        stage('Git Pulling') {
            steps {
                git branch: 'master', url: 'https://github.com/chebolunaveen/terraform-test.git'
            }
        }
        stage('Init') {
            steps {
                withAWS(credentials: 'aws-key', region: 'us-east-1') {
                sh 'terraform  init'
                }
            }
        }
        stage('Validate') {
            steps {
                withAWS(credentials: 'aws-key', region: 'us-east-1') {
                sh 'terraform  validate'
                }
            }
        }
        stage('Action') {
            steps {
                withAWS(credentials: 'aws-key', region: 'us-east-1') {
                    script {    
                        if (params.Terraform_Action == 'plan') {
                            sh "terraform  plan "
                        }   else if (params.Terraform_Action == 'apply') {
                            sh "terraform  apply  -auto-approve"
                        }   else if (params.Terraform_Action == 'destroy') {
                            sh "terraform -chdir=eks/ destroy  -auto-approve"
                        } else {
                            error "Invalid value for Terraform_Action: ${params.Terraform_Action}"
                        }
                    }
                }
            }
        }
    }
}
