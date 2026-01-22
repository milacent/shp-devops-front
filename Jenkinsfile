pipeline {
    agent {
        docker {
            image 'node:20'
            args '-u root'
        }
    }
    environment {
        PROJECT_DIR = '/var/www/shp-devops-front'
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Production') {
            steps {
                sh 'npm run build_prod'
            }
        }
        stage('Deploy to Production') {
            steps {
                sh '''
                    mkdir -p $PROJECT_DIR
                    
                    rm -rf $PROJECT_DIR/*
                    
                    cp -r dist/* $PROJECT_DIR/
                    
                    chown -R www-data:www-data $PROJECT_DIR || true
                    chmod -R 755 $PROJECT_DIR
                    
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
