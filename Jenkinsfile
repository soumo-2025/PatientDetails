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
                    sh 'pm2 stop todos-app'
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
