pipeline {
  agent any

  stages {
    stage('Checkout source code') {
      steps {
        checkout scm
      }
    }

    stage ('Install dependencies') {
      tools {
        // nodejs 'nodejs 8.9.4'
        nodejs 'nodejs 12.0.0'
      }
      steps {
        sh '''
          echo "Installing..."
          npm install
          echo "Install dependencies successfully."
          ls -al
        '''
      }
    }
    
    stage ('Test') {
      tools {
        nodejs 'nodejs 8.9.4'
      }
      steps {
        nodejs(nodeJSInstallationName: 'nodejs 8.9.4') {
          sh '''
            echo "Run unit test..."
            npm test
            echo "Run unit test successfully."
          '''
        }
      }
    }

    stage ('Build') {
      tools {
        nodejs 'nodejs 8.9.4'
      }
      steps {
          sh 'echo "Build application..."'
          sh 'npm run build'
          sh 'echo "Build application successfully."'
          sh 'ls -al'
      }
    }

    stage ('Create docker images') {
      agent any
      steps {
        sh '''
          ls -al
          echo "Starting to build docker image"
          docker build -t pick-color:v1 -f docker/Dockerfile .
        '''
      }
    }
  }
}