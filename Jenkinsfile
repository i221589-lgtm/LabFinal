pipeline {
    agent any

    environment {
        VENV = "venv"
        DEPLOY_DIR = "/tmp/flask_deploy"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/i221589-lgtm/LabFinal.git'
              echo "cloning repo" 
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
              echo "installing dependencies" 
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                . $VENV/bin/activate
                pytest
                '''
              echo "testing" 
            }
        }

        stage('Build Application') {
            steps {
                sh '''
                mkdir -p build
                cp -r app.py requirements.txt build/
                '''
              echo "building" 
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                mkdir -p $DEPLOY_DIR
                cp -r build/* $DEPLOY_DIR/
                echo "Application deployed to $DEPLOY_DIR"
                '''
              echo "Deploying" 
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
