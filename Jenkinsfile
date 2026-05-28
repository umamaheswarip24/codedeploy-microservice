pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-2'
        APPLICATION_NAME = 'Jenkins-CodeDeploy-App'
        DEPLOYMENT_GROUP = 'Jenkins-Deployment-Group'
        S3_BUCKET = 'uma-codedeploy-artifacts-2026'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/umamaheswarip24/codedeploy-microservice.git'
            }
        }

        stage('Create Deployment Package') {
            steps {
                sh 'zip -r deployment.zip .'
            }
        }

        stage('Upload Artifact to S3') {
            steps {
                sh 'aws s3 cp deployment.zip s3://$S3_BUCKET/deployment.zip'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                aws deploy create-deployment \
                --application-name $APPLICATION_NAME \
                --deployment-group-name $DEPLOYMENT_GROUP \
                --s3-location bucket=$S3_BUCKET,bundleType=zip,key=deployment.zip
                '''
            }
        }
    }
}

