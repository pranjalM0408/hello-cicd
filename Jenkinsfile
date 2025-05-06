pipeline {
    agent any
 
    environment {
        AWS_REGION = 'us-east-1'
    }
 
    stages {
        stage('Clone Repo') {
            steps {
git branch: 'main', url: 'https://github.com/pranjalM0408/hello-cicd.git'
            }
        }
 
        stage('Download Terraform Code from S3') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        export AWS_REGION=us-east-1
 
aws s3 cp s3://vilgax04-bucket/terraform-code.zip .
                    '''
                }
            }
        }
 
        stage('Deploy Infra using Terraform') {
            steps {
                sh '''
unzip -o terraform-code.zip -d tfdir
                    cd tfdir
                    terraform init
                    terraform apply -auto-approve
                '''
            }
        }
 
        stage('Upload HTML to Hosting Bucket') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        export AWS_REGION=us-east-1
 
                        aws s3 cp index.html s3://vilgax-host-bucket/index.html 
                    '''
                }
            }
            stage('Show Static Website URL') {
                steps {
                    script {
                        def websiteUrl = "http://vilgax-host-bucket.s3-website-us-east-1.amazonaws.com"
                        echo "Your Static website is live at : ${websiteUrl}"
                    }
                }
        }
    }
}
