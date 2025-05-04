pipeline {
    agent any
    environment {
TERRAFORM_S3_BUCKET = "pranjal-static-site-dev2025" 
TF_ZIP = "jenkins-ec2.zip"                   
        AWS_REGION = "ap-south-1"
    }
 
    stages {
        stage('Checkout Code') {
            steps {
git url: 'https://github.com/pranjalM0408/hello-cicd.git'  // <-- apna GitHub username aur repo name daalo
            }
        }
 
        stage('Download Terraform Code from S3') {
            steps {
                script {
                    sh """
                    aws s3 cp s3://$TERRAFORM_S3_BUCKET/$TF_ZIP .
                    unzip -o $TF_ZIP -d jenkins-ec2
                    """
                }
            }
        }
 
        stage('Run Terraform') {
            steps {
                script {
                    dir('jenkins-ec2') {
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
