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
        stage('Clone From Git To Instance'){
            steps{
                sh 'rm -rf Code'
                sh 'git clone https://github.com/davidshn/Project08.git Code'
            }
        }
        stage("check that html is downloaded") {
            steps{ 
                script {
                    def scriptContent = '''
                        #!/bin/bash
                        if [ -f .htmlhintrc ]; then
                            echo "The .htmlhintrc file exists."
                        else
                            apt update -y
                            apt install npm -y
                            npm install --save-dev htmlhint
                            echo '{
    "tagname-lowercase": true,
    "attr-lowercase": true,
    "attr-value-double-quotes": true,
    "attr-no-duplication": true,
    "doctype-first": false,
    "tag-pair": true,
    "tag-self-close": true,
    "spec-char-escape": true,
    "id-unique": true,
    "src-not-empty": true,
    "attr-unsafe-chars": true
}' > .htmlhintrc
                        fi

                    '''
                    // Write the script content to a temporary file
                    def scriptFile = writeFile(file: 'install-htmlhint.sh', text: scriptContent)

                    // Make the script executable
                    sh 'chmod +x install-htmlhint.sh'

                    // Execute the script
                    sh './install-htmlhint.sh'

                    // Clean up the temporary script file if needed
                    sh 'rm -f install-htmlhint.sh'
                }
            }
        }

        stage("html local analasys") {
            steps{ 
                dir('/var/lib/jenkins/workspace/Port8/Code') {
                script {
                    def htmlhintResultHTML = sh(script: 'npx htmlhint "**/*.html"', returnStatus: true)
                    
                    if (htmlhintResultHTML != 0) {
                        error('There are errors in HTML files.')
                    }
                    def htmlhintResultCSS = sh(script: 'npx htmlhint "**/*.css"', returnStatus: true)
                    
                    if (htmlhintResultCSS != 0) {
                        error('There are errors in CSS files.')
                    }
                }
            }
          }
        }
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}" // specifing a version
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") { // accessing the server with token
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=port8 \
                   -Dsonar.projectName=port8 \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=/var/lib/jenkins/workspace/Port8/ '''
              }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Upload to S3') {
            steps {
                dir('/var/lib/jenkins/workspace/Port8') {
                script {
                    withAWS(region:"${BUCKET_REGION}", credentials:"${BUCKET_CREDENTIALS}") {
                        s3Upload(file:"${BUCKET_FILE}", bucket:"${BUCKET_NAME}", path:"${BUCKET_PATH}")
                    }
                  }
                }
            }
        }
         stage("Something") {
            steps {
                    echo 'skipped'
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



 


