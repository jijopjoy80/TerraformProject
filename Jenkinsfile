pipeline {
    agent any
    
    stages {
        stage('Terraform Init') {
            steps {
                // Initialize Terraform and set up the backend
                dir('/home/jenkins/terraform-aws') {
                    sh 'terraform init'
                }
            }
        }
        
        stage('Terraform Apply') {
            steps {
                // Apply the Terraform configuration and provision infrastructure resources without manual confirmation
                dir('/home/jenkins/terraform-aws') {
                    sh 'terraform apply -auto-approve'
                }
            }
        }
        
        stage('Wait') {
            steps {
                // Wait for some time (180 seconds)
                sh 'sleep 180'
            }
        }
        
        stage('Fetch and Save Content') {
            steps {
                // Fetch the content from the specified URL and save it to localfile.txt
                dir('/home/jenkins/terraform-aws') {
                    // Get the instance public IP from Terraform output
                    def INSTANCE_IP
                    script {
                        INSTANCE_IP = sh(returnStdout: true, script: 'terraform output -raw instance_public_ip').trim()
                    }
                    // Use curl to fetch the content and save it to localfile.txt
                    sh "curl http://${INSTANCE_IP}:80 > localfile.txt"
                }
            }
        }
        
        stage('Display File Contents') {
            steps {
                // Display the contents of the localfile.txt file
                dir('/home/jenkins/terraform-aws') {
                    sh 'cat localfile.txt'
                }
            }
        }
        
        stage('Terraform Destroy') {
            steps {
                // Destroy the infrastructure resources provisioned by Terraform without manual confirmation
                dir('/home/jenkins/terraform-aws') {
                    sh 'terraform destroy -auto-approve'
                }
            }
        }
    }
}
