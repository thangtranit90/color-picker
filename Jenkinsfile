pipeline {
    agent any

    tools {
        nodejs 'NodeJS 8.9.4'
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh '''
                    echo "Installing..."
                    npm install
                    echo "Install dependencies successfully."
                    ls -al
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Build application..."
                    npm run build
                    echo "Build application successfully."
                    ls -al
                '''
                script {
                    stash includes: 'build/', name: 'build'
                    stash includes: 'docker/', name: 'docker_folder'
                }
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "Run unit test..."
                    npm test
                    echo "Run unit test successfully."
                '''
            }
        }

        stage('Create docker images') {
            steps {
                script {
                    unstash 'build'
                    unstash 'docker_folder'
                }
                sh '''
                    ls -al
                    echo "Starting to build docker image"
                    docker build -t pick-color:v1 -f docker/Dockerfile .
                '''
            }
        }
    }
}
