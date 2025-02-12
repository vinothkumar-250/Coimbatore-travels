pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/vinothkumar-250/Coimbatore-travels.git'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo Deploying to AWS Elastic Beanstalk...'
            }
        }
    }
}
