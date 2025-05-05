pipeline {
    agent any
 
    environment {
        TERRAFORM_BUCKET = 'vilgax10-bucket'
        STATIC_SITE_BUCKET = 'vilgax-host-bucket'
ZIP_FILE = 'terraform-code.zip'
    }
 
    stages {
        stage('Clone HTML Repo') {
            steps {
git 'https://github.com/pranjalM0408/hello-cicd.git'
            }
        }
 
        stage('Download Terraform Code from S3') {
            steps {
                sh '''
                    aws s3 cp s3://$TERRAFORM_BUCKET/$ZIP_FILE .
                    unzip -o $ZIP_FILE -d tfdir
                '''
            }
        }
 
        stage('Deploy Infra using Terraform') {
            steps {
                sh '''
                    cd tfdir
                    terraform init
                    terraform apply -auto-approve
                '''
            }
        }
 
        stage('Upload index.html to Hosting Bucket') {
            steps {
                sh '''
                    aws s3 cp index.html s3://$STATIC_SITE_BUCKET/index.html --acl public-read
                '''
            }
        }
    }
}
