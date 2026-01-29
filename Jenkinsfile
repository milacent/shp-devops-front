pipeline {
    agent any
    
    environment {
        PROJECT_DIR = '/var/www/shp-devops-front/'
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
		    mkdir -p ${PROJECT_DIR}        
                    cp -r dist/* ${PROJECT_DIR}/
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
