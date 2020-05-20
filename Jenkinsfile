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

        stage('Site is Live') {
            steps {
                sh 'echo "Checking if Site is Live..."'
                sh '''
                        #!/bin/bash
                        url='http://jenkins-imyke.s3-website.us-west-2.amazonaws.com'
                        attempts=5
                        timeout=5
                        online=false

                        echo "Checking status of $url."

                        for (( i=1; i<=$attempts; i++ ))
                        do
                            code=`curl -sL --connect-timeout 20 --max-time 30 -w "%{http_code}\\n" "$url" -o /dev/null`

                            echo "Found code $code for $url."

                            if [ "$code" = "200" ]; then
                                echo "Website $url is online."
                                online=true
                                break
                            else
                                echo "Website $url seems to be offline. Waiting $timeout seconds."
                                sleep $timeout
                            fi
                        done

                        if $online; then
                            echo "Monitor finished, website is online."
                            exit 0
                        else
                            echo "Monitor failed, website seems to be down."
                            exit 1
                        fi
                    '''
            }
        }
    }
}