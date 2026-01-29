pipeline {
    agent {
        docker {
            image 'node:18'
            args '-u root -v /var/www:/var/www -v /etc/nginx/sites-available:/etc/nginx/sites-available -v /etc/nginx/sites-enabled:/etc/nginx/sites-enabled'
        }
    }

    environment {
        PROD_DIR = '/var/www/shp-devops-front/'
        CONF_PATH = '/etc/nginx/sites-available/kopeykina-front.conf'
        CONF_NAME = 'kopeykina-front.conf'
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build production') {
            steps {
                sh 'npm run build_prod'
            }
        }

        stage('Deploy to production') {
            steps {
                sh '''
                    set -e

                    mkdir -p ${PROD_DIR}
                    rm -rf ${PROD_DIR}/*
                    cp -r dist/* ${PROD_DIR}/

                    cat > ${CONF_PATH} <<'EOF'
server {
    listen 443 ssl;
    server_name kopeykina.mshptop.ru;

    location / {
        root /var/www/shp-devops-front;
        try_files $uri /index.html;
    }

    ssl_certificate /etc/letsencrypt/live/kopeykina.mshptop.ru/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/kopeykina.mshptop.ru/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
}
EOF

                    ln -sf ${CONF_PATH} /etc/nginx/sites-enabled/${CONF_NAME}
                '''
            }
        }
    }
}