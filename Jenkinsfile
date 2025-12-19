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

                        // ✅ Absolute path check
                        def terraformCheck = bat(
                            script: 'C:\\terraform\\terraform.exe -version',
                            returnStatus: true
                        )

                        if (terraformCheck != 0) {
                            error "Terraform executable not found at C:\\terraform\\terraform.exe"
                        }

                        echo "Terraform found. Running Init, Plan, Apply..."

                        dir('Terraform') {
                            bat 'C:\\terraform\\terraform.exe init'
                            bat 'C:\\terraform\\terraform.exe plan'
                            bat 'C:\\terraform\\terraform.exe apply -auto-approve'
                        }

                        echo "Terraform execution completed successfully."
                    }
                }
            }
        }
    }
}
