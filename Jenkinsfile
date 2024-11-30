pipeline {
    agent any
    tools {
        maven 'Maven_3.8.5' 
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/your-repo.git'  
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'  
            }
        }
        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'  
                }
            }
        }
        stage('Upload Artifact') {
            steps {
                sh 'aws s3 cp target/your-app.jar s3://your-bucket-name/'  // Replace with your AWS bucket
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['your-ssh-credentials-id']) {
                    sh '''
                    ssh ec2-user@your-ec2-instance "sudo systemctl restart your-app-service"  // Replace with your EC2 instance details
                    '''
                }
            }
        }
    }
    post {
        always {
            mail to: yashgautamsgs@gmail.com,  // Replace with your email
                 subject: "Pipeline Status: ${currentBuild.currentResult}",
                 body: "The pipeline finished with status ${currentBuild.currentResult}."
        }
    }
}
