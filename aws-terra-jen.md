```

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // 1. Checkout the code first
                git branch: 'main', url: 'https://github.com/mhprasanna-spec/for_Jenkins.git'
                
                // 2. Wrap terraform commands inside the credentials block
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                                    credentialsId: 'ubuntu', 
                                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        terraform init
                        terraform plan
                        terraform apply --auto-approve
                    '''
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }




    }
}



```


## syntax pipeline (dec)


