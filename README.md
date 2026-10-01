# DevOps Practical Test

This is the source code for the DevOps practical test.

## Project Structure
- `server.js`: Main Express application.
- `package.json`: Project dependencies and scripts.
- `Jenkinsfile`: Jenkins pipeline configuration for CI/CD.

## Requirements

1. **GitHub**: Push this repository to GitHub under the name `devops-test-thien`.
2. **Jenkins**: Set up a pipeline connecting to the GitHub repository.
3. **CI/CD**: Configure Jenkins to build and deploy automatically upon pushing to the `main` branch.
4. **Telegram**: Configure Telegram notifications for Deploy Started, Deploy Success, and Deploy Failed.

## How to run locally
1. Install dependencies: `npm install`
2. Run the application: `npm start`
3. The server will run on `http://localhost:3000`

## Jenkins Configuration
- Add `telegram_bot_token` and `telegram_chat_id` as Secret Text in Jenkins Credentials.
- Install Node.js and npm on your Jenkins server/agent.
- Ensure `pm2` is installed globally (`npm install -g pm2`) to handle deployments.
- Set up GitHub Webhooks to trigger the Jenkins pipeline automatically on pushes to `main`.
