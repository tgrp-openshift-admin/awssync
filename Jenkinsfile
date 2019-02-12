pipeline{
    agent any
    environment {
        // TODO:password
        AWS_ACCESS_KEY_ID = credentials("${params.ACCESS_KEY_CREDENTIAL}")
        AWS_SECRET_ACCESS_KEY = credentials("${params.SECRET_ACCESS_KEY_CREDENTIAL}")
    }
    stages {
        stage('sync'){
            steps {
                sh "/var/lib/jenkins/aws s3 sync s3://${params.S3BUCKET_FROM}/ s3://${params.S3BUCKET_TO}/ --exact-timestamp --delete --quiet"
            }
        }
        stage('invalidation'){
            steps {
                sh "/var/lib/jenkins/aws cloudfront create-invalidation --distribution-id ${params.CLOUDFRONT_DISTRIBUTION} --paths '/*'"
            }
        }
    }
    post {
        success {
            echo 'Success!'
        }
        failure {
            echo 'Failure...'
        }
    }
}
