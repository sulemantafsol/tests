pipeline {
    agent any

    tools {
        nodejs 'NodeJS18' // Use the name you configured in Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies (Backend)') {
            steps {
                sh 'cd server && npm install' // Or 'yarn install'
            }
        }

        stage('Install Dependencies (Frontend)') {
            steps {
                sh 'cd client && npm install' // Or 'yarn install'
            }
        }

        stage('Linting (Backend)') {
            steps {
                sh 'cd server && npm run lint' // Replace 'lint' with your linting script
            }
        }

        stage('Linting (Frontend)') {
            steps {
                sh 'cd client && npm run lint' // Replace 'lint' with your linting script
            }
        }

        stage('Testing (Backend)') {
            steps {
                sh 'cd server && npm test' // Replace 'test' with your testing script
            }
        }

        stage('Testing (Frontend)') {
            steps {
                sh 'cd client && npm test' // Replace 'test' with your testing script
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'cd client && npm run build' // Replace 'build' with your build script
            }
        }

        stage('Package Application') {
            steps {
                sh 'mkdir -p build-output'
                sh 'cp -r server build-output/'
                sh 'cp -r client/build build-output/client' // Copy the built frontend
                // Optional: You might want to create a compressed archive here
            }
        }

        stage('Deploy') {
            steps {
                // Deployment steps will vary greatly depending on your target environment.
                // Here are some common scenarios:

                // 1. Deploying to the same Ubuntu VM (for testing/development):
                sh 'echo "Deploying to the local VM..."'
                sh 'sudo systemctl stop your-mern-app.service || true' // Stop existing service (if using systemd)
                sh 'rm -rf /opt/mern-app/*' // Clear previous deployment
                sh 'cp -r build-output/* /opt/mern-app/' // Copy new build
                sh 'cd /opt/mern-app/server && npm install --production' // Install production backend dependencies
                sh 'sudo systemctl start your-mern-app.service' // Start the service

                // 2. Deploying to a remote server (you'll need SSH access):
                /*
                withCredentials([sshUserPrivateKey(credentialsId: 'your-ssh-key-id', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                        echo "Deploying to remote server..."
                        ssh -o StrictHostKeyChecking=no -i "$SSH_PRIVATE_KEY" user@your-remote-ip "
                            mkdir -p /path/to/deployment
                            rm -rf /path/to/deployment/*
                            cd /path/to/deployment/server && npm install --production
                            # Start/restart your application (e.g., using pm2, systemd)
                            # pm2 restart your-app
                        "
                        scp -o StrictHostKeyChecking=no -i "$SSH_PRIVATE_KEY" -r build-output/* user@your-remote-ip:/path/to/deployment
                    '''
                }
                */

                echo 'Deployment steps configured in Jenkinsfile'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
