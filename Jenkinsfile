def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

pipeline{
    agent any
    environment {
        SONARSERVER = 'sonarserver' // from system, name of installations
        SONARSCANNER = 'sonarserver' //from tools, name of scanner
        BUCKET_REGION = 'us-east-2'
        BUCKET_CREDENTIALS = 's3-log-admin'
        BUCKET_FILE = 'Code'
        BUCKET_NAME = 'port8-bucket'
        BUCKET_PATH = 'Code'
    }
    stages{
         stage("Download from s3 to local") {
            steps {
                dir('/var/lib/jenkins/workspace/Port8') {
                script{
                    sh " aws s3 cp s3://port8-bucket/Code ."
                }
             }
            }
        }
    }
    post {
        always { // even if pipeline fails - this runs
            echo 'Slack Notifications.' 
            slackSend channel: '#port-8',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* \n Job: ${env.JOB_NAME} Build: ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    
   }
}



 


