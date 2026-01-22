pipeline {
    agent {
        docker {
            image 'node:20'
            args '-u root'
        }
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
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
        }
    }
}
