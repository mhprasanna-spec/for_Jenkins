CI/CD Pipeline: Java Artifact Deployment to AWS S3
This documentation outlines the process of building a Java application using Maven and deploying the resulting .jar artifact to an Amazon S3 bucket using a Jenkins Pipeline.
Prerequisites
1. Infrastructure & Tools
EC2 Instance: Running with Java, Maven, and Jenkins installed.
AWS S3 Bucket:
Name: newb65buxx
Region: us-east-1
2. Jenkins Plugins
The following plugins must be installed via Manage Jenkins > Plugins:
Pipeline
S3 Publisher / Artifact Manager on S3
AWS Steps
Configuration Steps
1. AWS Credentials in Jenkins
Go to Manage Jenkins > Credentials.
Add a new credential of type AWS Credentials.
Input your Access Key ID and Secret Access Key.
Set the ID as aws-credentials-id (used in the pipeline script).
2. System Configuration
Navigate to Manage Jenkins > System.
Find the Amazon S3 Profiles section.
Add a profile using the credentials created above to ensure Jenkins can communicate with your S3 bucket.
Pipeline Workflow
The pipeline is designed to perform the following stages:
Checkout: Pulls the source code from the repository.
Build: Navigates to the backend directory and runs Maven.
Command: mvn clean package -DskipTests
Deploy: Uploads the generated .jar file from the target/ directory to the S3 bucket.
Example Jenkinsfile
groovy
pipeline {
    agent any
    
    environment {
        BUCKET_NAME = 'newb65buxx'
        REGION = 'us-east-1'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull your source code
                checkout scm
            }
        }

        stage('Build') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-credentials-id', region: "${REGION}") {
                    // Upload the jar file to S3
                    s3Upload(
                        bucket: "${BUCKET_NAME}",
                        workingDir: 'backend/target',
                        includePathPattern: '**/*.jar'
                    )
                }
            }
        }
    }
}
Use code with caution.

Verification
After the pipeline runs successfully:
Log in to the AWS Management Console.
Navigate to the S3 service.
Open the newb65buxx bucket to confirm the .jar file is present.
