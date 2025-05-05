pipeline {
    agent any
    environment {
        
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')    // Replace with your Jenkins credential ID
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key') // Replace with your Jenkins credential ID
    
TERRAFORM_S3_BUCKET = "pranjal-static-site-dev2025" 
TF_ZIP = "jenkins-ec2.zip"                   
        AWS_REGION = "us-east-1"
    }
 
    stages {
        stage('Checkout Code') {
            steps {
git url: 'https://github.com/pranjalM0408/hello-cicd.git', branch: 'main'  // <-- apna GitHub username aur repo name daalo
            }
        }
 
        stage('Download Terraform Code from S3') {
            steps {
                script {
                    sh """
                    wget
                    https://pranjal-static-site-dev2025.s3.amazonaws.com/jenkins-ec2.zip
                    unzip -o $TF_ZIP -d jenkins-ec2.zip
                    """
                }
            }
        }
 
        stage('Run Terraform') {
            steps {
                script {
                    dir('jenkins-ec2.zip') {
                        sh 'terraform init'
                        sh 'terraform apply -auto-approve'
                    }
                }
            }
        }
    }
 
    post {
        always {
            echo "Pipeline executed successfully"
        }
    }
}
