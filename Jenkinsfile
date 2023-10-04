pipeline{
    agent any
    stages{
        stage('Clone From Git To Instance'){
            steps{
                sh 'rm -rf Code'
                sh 'git clone https://github.com/davidshn/Project08.git Code'
            }
        }
        stage("Build") {
            steps {
                git url: 'https://github.com/davidshn/Project08.git'
                withMaven {
                sh "mvn clean verify"
                }
            }
        }


    }

}
