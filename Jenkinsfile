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
    }
}
