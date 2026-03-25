pipeline {
    agent any

    stages {
        stage('Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/mhprasanna-spec/EasyCRUD-Updated_k8s.git'
            }
        }
        stage('Test') {
            steps {
                sh '''
                    cd backend
                    mvn clean package -DskipTests
                '''
            }
        }
        stage('s3 upload') {
            steps {
                // Move withCredentials INSIDE steps
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                                     credentialsId: 'ubuntu', 
                                     secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    
                    // Removed empty/conflicting parameters
                    s3Upload(
                        bucket: 'newb65buxx',
                        file: 'backend/target/student-registration-backend-0.0.1-SNAPSHOT.jar',
                        acl: 'Private'
                    )
                }
            }
        }
    }
}

