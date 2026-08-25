pipeline {
    agent {
        label 'uat-worker'
    }

    stages {

        stage('Build Frontend') {
            steps {
                sh '''
                    npm ci
                    CI=false REACT_APP_API_USE_PROXY=true npm run build
                '''
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                    sudo mkdir -p /var/www/fusion-app
                    sudo rm -rf /var/www/fusion-app/*
                    sudo cp -r build/* /var/www/fusion-app/
                '''
            }
        }

        stage('Deploy Backend') {
            steps {
                sh '''
                    cd backend
                    npm ci

                    pm2 delete fusion-backend || true
                    pm2 start index.js --name fusion-backend
                    pm2 save
                '''
            }
        }
    }
}
