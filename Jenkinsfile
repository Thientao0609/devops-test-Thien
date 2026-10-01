def notify(String msg) {
    bat "curl -s -X POST https://api.telegram.org/bot%TELEGRAM_BOT_TOKEN%/sendMessage -d chat_id=%TELEGRAM_CHAT_ID% -d \"text=${msg}\" -d parse_mode=Markdown"
}

pipeline {
    agent any

    triggers { githubPush() }

    environment {
        TELEGRAM_BOT_TOKEN = credentials('telegram-token')
        TELEGRAM_CHAT_ID   = credentials('telegram-chat-id')
        APP_URL = "http://localhost:3000"
    }

    stages {
        stage('Notify Start') {
            steps {
                script {
                    notify("%%F0%%9F%%9A%%80 *DEPLOY STARTED*%%0AProject: devops-test%%0ABranch: main")
                }
            }
        }
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Install Dependencies') {
            steps { bat 'npm install' }
        }
        stage('Build') {
            steps {
                // nếu package.json có script build thì đổi thành: bat 'npm run build'
                bat 'node --check src/server.js'
            }
        }
        stage('Deploy') {
            steps {
                bat 'pm2 restart devops-test || pm2 start src/server.js --name devops-test'
            }
        }
    }

    post {
        success {
            script {
                notify("%%E2%%9C%%85 *DEPLOY SUCCESS*%%0AProject: devops-test%%0ABranch: main%%0AURL: ${env.APP_URL}")
            }
        }
        failure {
            script {
                notify("%%E2%%9D%%8C *DEPLOY FAILED*%%0AProject: devops-test%%0ABranch: main%%0APlease check Jenkins.")
            }
        }
    }
}