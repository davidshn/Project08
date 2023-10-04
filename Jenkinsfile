pipeline{
    agent any
    stages{
        stage('Clone From Git To Instance'){
            steps{
                sh 'rm -rf ~/Code'
                sh 'git clone -f https://github.com/davidshn/Project08.git Code'
                sh 'mv Code `/'
                sh 'echo "done"'
                echo 'cuntys'
            }
        }
    }

}
