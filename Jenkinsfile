pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                echo "Multiline shell steps works too"
                ls -lah
                '''
            }
        }
        stage('Lint HTML') {
            when {
                branch 'Development' 
            }
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Security Scan') {
            when {
                branch 'Development' 
            }
            steps { 
                aquaMicroscanner imageName: 'alpine:latest', notCompleted: 'exit 1', onDisallowed: 'fail'
            }
        }         
        stage('Upload to AWS') {
            when {
                branch 'Deployment'
            }
            steps {
                withAWS(region:'us-east-2',credentials:'aws-static') {
                    sh 'echo "Uploading content with AWS creds"'
                    s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-jenkins-pipeline')
                }
            }
        }
    }
}
