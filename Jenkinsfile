pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/vinothkumar-250/Coimbatore-travels.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building project...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Ready to deploy to AWS Elastic Beanstalk...'
            }
        }
    }
}
pipeline {
    agent any
    environment {
        AWS_REGION = 'eu-north-1'  
        S3_BUCKET = 'test-bucket-98941'
        EB_APP_NAME = 'jenkins_test'
        EB_ENV_NAME = 'jenkinstest-env'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/vinothkumar-250/Coimbatore-travels.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo 'Building project...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Deploy to AWS Elastic Beanstalk') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    script {
                        def zipFile = "app-${env.BUILD_NUMBER}.zip"
                        sh "zip -r ${zipFile} *"
                        sh "aws s3 cp ${zipFile} s3://${env.S3_BUCKET}/${zipFile}"
                        sh "aws elasticbeanstalk create-application-version --application-name ${env.EB_APP_NAME} --version-label v${env.BUILD_NUMBER} --source-bundle S3Bucket=${env.S3_BUCKET},S3Key=${zipFile}"
                        sh "aws elasticbeanstalk update-environment --environment-name ${env.EB_ENV_NAME} --version-label v${env.BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}
