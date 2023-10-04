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
                def REQUIRED_PKG=  'some-package'
                def PKG_OK = sh(script: "dpkg-query -W --showformat='\${Status}\n' $REQUIRED_PKG | grep 'install ok installed'")
                echo "Checking for $REQUIRED_PKG: $PKG_OK"
                if [ "" = "$PKG_OK" ]; then
                    echo "No $REQUIRED_PKG. Setting up $REQUIRED_PKG."
                    sh 'sudo apt-get --yes install $REQUIRED_PKG'
                fi
                }
            }
        }


    }


