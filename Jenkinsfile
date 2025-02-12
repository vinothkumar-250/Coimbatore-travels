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
                        def buildNumber = env.BUILD_NUMBER ?: new Date().format('yyyyMMddHHmmss')
                        def zipFile = "app-${buildNumber}.zip"
                        def versionLabel = "v${buildNumber}"
                        
                        sh "zip -r ${zipFile} . -x '*.git*'"
                        sh "ls -lh ${zipFile}" // Debugging

                        sh "aws s3 cp ${zipFile} s3://${S3_BUCKET}/${zipFile} --debug"

                        sh """
                            aws elasticbeanstalk create-application-version \
                            --application-name ${EB_APP_NAME} \
                            --version-label ${versionLabel} \
                            --source-bundle S3Bucket=${S3_BUCKET},S3Key=${zipFile} \
                            --debug
                        """

                        sh """
                            aws elasticbeanstalk update-environment \
                            --environment-name ${EB_ENV_NAME} \
                            --version-label ${versionLabel}
                        """
                    }
                }
            }
        }
    }
}
