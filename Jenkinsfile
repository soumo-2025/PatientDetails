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
stage('Test the app') {
            steps {
                script {
                    sh 'curl http://localhost:3000/health'
                }
            }
        }
    stage('Stop the App') {
            steps {
                script {
                    sh 'pm2 stop PatientDetails'
                }
            }
        }  

        stage('Add Host to known_hosts') {
            steps {
                script {
                    sh '''
                        ssh-keyscan -H $PRODUCTION_IP_ADRESSS >> /var/lib/jenkins/.ssh/known_hosts
                    '''
                }
            }
        }

        stage('Deploy') {
                environment {
                    DEPLOY_SSH_KEY = credentials('AWS_INSTANCE_SSH')
                }
        }
}
}
 steps {
                    sh '''
                        ssh -v -i $DEPLOY_SSH_KEY ubuntu@$PRODUCTION_IP_ADRESSS '
                            
                            if [ ! -d "PatientDetails" ]; then
                                git clone https://github.com/soumo-2025/PatientDetails.git PatientDetails
                                cd todos-app
                            else
                                cd PatientDetails
                                git pull
                            fi

                            yarn install
                            
                            if pm2 describe [Patientdetails > /dev/null ; then
                            pm2 restart PatientDetails
                            else
                                yarn start:pm2
                            fi
                        '
                    '''
                }
            }
    }
}
