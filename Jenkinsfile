pipeline{
    agent any
    environment {
        SONARSERVER = 'sonarserver' // from system, name of installations
        SONARSCANNER = 'sonarserver' //from tools, name of scanner
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
                            echo "The .htmlhintrc file exists in the current directory."
                        else
                            apt update -y
                            apt install npm -y
                            npm install --save-dev htmlhint
                            echo '{ "attr-value-not-empty": false }' > .htmlhintrc
                            npx htmlhint "**/*.html"
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
                    def htmlhintResult = sh(script: 'npx htmlhint "**/*.html"', returnStatus: true)
                    
                    if (htmlhintResult != 0) {
                        error('HTMLHint found errors in HTML files. Failing the pipeline.')
                    }
                    def htmlhintResult = sh(script: 'npx htmlhint "**/*.css"', returnStatus: true)
                    
                    if (htmlhintResult != 0) {
                        error('HTMLHint found errors in CSS files. Failing the pipeline.')
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
    }


 }


