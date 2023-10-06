pipeline{
    agent any
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
        stage("sonarqube") {
            steps{ 
                echo 'skipped'
            }
        }
    }


 }


