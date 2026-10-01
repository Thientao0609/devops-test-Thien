pipeline {
    agent any

    environment {
        // You should configure these in Jenkins credentials as Secret text
        TELEGRAM_BOT_TOKEN = credentials('telegram_bot_token')
        TELEGRAM_CHAT_ID = credentials('telegram_chat_id')
        APP_URL = "http://localhost:3000" 
    }

    stages {
        stage('Notify Start') {
            steps {
                script {
                    def msg = "🚀 *DEPLOY STARTED* %0AProject: devops-test %0ABranch: ${env.BRANCH_NAME} %0A"
                    bat "curl -s -X POST https://api.telegram.org/bot%TELEGRAM_BOT_TOKEN%/sendMessage -d chat_id=%TELEGRAM_CHAT_ID% -d text=\"${msg}\" -d parse_mode=Markdown"
                }
            }
        }
        
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                // A simple command just to simulate a build step
                bat 'echo "Build step completed"'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    // We use pm2 to run the Node.js server. If pm2 is not installed, install it: npm i -g pm2
                    bat 'pm2 restart devops-test || pm2 start src/server.js --name "devops-test"'
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished with status: ${currentBuild.currentResult}"
        }
        success {
            script {
                def msg = "✅ *DEPLOY SUCCESS* %0AProject: devops-test %0ABranch: ${env.BRANCH_NAME} %0AURL: ${APP_URL}"
                bat "curl -s -X POST https://api.telegram.org/bot%TELEGRAM_BOT_TOKEN%/sendMessage -d chat_id=%TELEGRAM_CHAT_ID% -d text=\"${msg}\" -d parse_mode=Markdown"
            }
        }
        failure {
            script {
                def msg = "❌ *DEPLOY FAILED* %0AProject: devops-test %0ABranch: ${env.BRANCH_NAME} %0APlease check Jenkins."
                bat "curl -s -X POST https://api.telegram.org/bot%TELEGRAM_BOT_TOKEN%/sendMessage -d chat_id=%TELEGRAM_CHAT_ID% -d text=\"${msg}\" -d parse_mode=Markdown"
            }
        }
    }
}
