pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                sh 'ls -la'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying..."
            }
        }
    }
}
