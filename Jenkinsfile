pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        S3_BUCKET = '001002003004005'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Jaykisen/particle-static-site.git'
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials-id' // Replace with your Jenkins AWS credentials ID
                ]]) {
                    sh '''
                    echo "Deploying to S3..."
                    aws s3 sync . s3://$S3_BUCKET --delete --region $AWS_REGION --exclude ".git/*" --exclude "Jenkinsfile"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment to S3 successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
