pipeline{
    agent any
    stages{
        stage('Clone From Git To Instance'){
            steps{
                sh 'rm -rf Code'
                sh 'git clone https://github.com/davidshn/Project08.git Code'
            }
        }
        stage("Check Apache2") {
            steps {
                script {
                    def scriptContent = '''
                        #!/bin/bash
                        REQUIRED_PKG="apache2"
                        PKG_OK=$(dpkg-query -W --showformat='${Status}\n' $REQUIRED_PKG|grep "install ok installed")
                        echo Checking for $REQUIRED_PKG: $PKG_OK
                        if [ "" = "$PKG_OK" ]; then
                            echo "No $REQUIRED_PKG. Setting up $REQUIRED_PKG."
                            sudo apt-get --yes install $REQUIRED_PKG
                        fi
                    '''

                    // Write the script content to a temporary file
                    def scriptFile = writeFile(file: 'install-apache2.sh', text: scriptContent)

                    // Make the script executable
                    sh 'chmod +x install-apache2.sh'

                    // Execute the script
                    sh './install-apache2.sh'

                    // Clean up the temporary script file if needed
                    sh 'rm -f install-apache2.sh'
                }
                }
            }
        stage('Push Code to Apache2'){
            steps{
                sh 'sudo mv Code/* /var/www/html'
            }
        }
        stage('Start n Enable Apache2'){
            steps{
                sh "sudo systemctl start apache2"
                sh "sudo systemctl enable apache2"
            }
        }
        stage('Allow Port 80'){
            steps{
                sh "sudo ufw allow 'Apache'"
                sh "sudo ufw enable"
            }
        }
    }


 }


