pipeline {
    agent any

     tools {
        nodejs "nodejs"
    }
stages {
        stage('Install Packages') {
            steps {
                script {
                    sh 'yarn install'
                }
            }
        }


   stage('Run the App') {
            steps {
                script {
                    sh 'yarn start &'
                    sleep 5
                }
            }
        }     
}
}
