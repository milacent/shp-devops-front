pipeline {
    agent any
    
    environment {
        PROJECT_DIR = '/var/www/shp-devops-front'
    }
    
    stages {
        stage('Install Node.js Dependencies') {
            agent {
                docker {
                    image 'node:20'
                    args '-u root'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Build Production') {
            agent {
                docker {
                    image 'node:20'
                    args '-u root'
                }
            }
            steps {
                sh 'npm run build_prod'
            }
        }
        stage('Deploy to Production') {
            steps {
                sh '''

                    sudo mkdir -p $PROJECT_DIR
                    
                    sudo rm -rf $PROJECT_DIR/*
                    
                    sudo cp -r dist/* $PROJECT_DIR/
                    
                    sudo chown -R www-data:www-data $PROJECT_DIR
                    sudo chmod -R 755 $PROJECT_DIR
                    
                    echo " Deployed to $PROJECT_DIR"
                    ls -la $PROJECT_DIR
                '''
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
        }
    }
}
