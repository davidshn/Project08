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
        stage("optimazion of pictures") {
            steps{ 
                echo 'skipped'
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


