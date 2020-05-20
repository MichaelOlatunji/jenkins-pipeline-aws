pipeline {
    agent any
    stages {
        stage('Lint HTML'){
            steps {
                sh 'echo "Linting HTML"'
                sh 'tidy -q -e *.html'
            }
        }

        stage('Upload to AWS') {
            steps {
                withAWS(region: 'us-west-2', credentials: 'aws-access-token'){
                    sh '''
                
                    echo "Uploading content to AWS S3"
                    echo "S3 Bucket Name: jenkins-imyke"
                    echo "Hosts a static website"

                    '''
                    s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, bucket: 'jenkins-imyke', file: 'index.html')
                }
                
            }
        }
    }
}