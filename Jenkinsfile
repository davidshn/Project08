pipeline{
    agent any
    stages{
        stage('Clone From Git To Instance'){
            steps{
                sh 'rm -rf $HOME/Code'
                sh 'git clone --separate-git-dir=$HOME/.mygit https://github.com/davidshn/Project08.git $HOME/Code'
            }
        }
    }

}
