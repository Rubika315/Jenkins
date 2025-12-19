pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-west-1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
                git branch: 'main',
                    url: 'https://github.com/Rubika315/Jenkins.git'
            }
        }

        stage('Terraform Steps') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                    script {
                        echo "Checking if Terraform is installed..."
                        def terraformCheck = bat(script: 'terraform -version', returnStatus: true)
                        if (terraformCheck != 0) {
                            error "Terraform is not installed or not in PATH!"
                        }

                        echo "Terraform is installed. Running Init, Plan, Apply..."

                        // Run Terraform inside the terraform folder
                        dir('Terraform') {
                            bat 'terraform init'
                            bat 'terraform plan'
                            bat 'terraform apply -auto-approve'
                        }

                        echo "Terraform execution completed successfully."
                    } // closes script
                } // closes withCredentials
            } // closes steps
        } // closes stage

    } // closes stages
} // closes pipeline
