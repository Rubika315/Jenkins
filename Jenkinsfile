pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-west-1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Rubika315/Jenkins.git', branch: 'main'
            }
        }

        stage('Terraform Init') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                    bat '''
                        cd terraform
                        terraform init
                    '''
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                    bat '''
                        cd terraform
                        terraform plan
                    '''
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                    bat '''
                        cd terraform
                        terraform apply -auto-approve
                    '''
                }
            }
        }
    }
}
