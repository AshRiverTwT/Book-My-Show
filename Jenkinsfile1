pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        DOCKER_IMAGE = "kalyantwt/book-my-show"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code from GitHub') {
            steps {
                git branch: 'feature-change',
                url: 'https://github.com/AshRiverTwT/Book-My-Show.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh """
                    ${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=BookMyShow \
                    -Dsonar.projectName=BookMyShow
                    """
                }
            }
        }
        
        stage('Install Dependencies (NPM)') {
            steps {
                dir('bookmyshow-app') {
                sh 'npm install'
                }
            }
        }
        
        stage('Trivy FS Scan') {
            steps {
                sh 'trivy fs .'
            }
        }



        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest ./bookmyshow-app'
            }
        }

        stage('Docker Push') {
            steps {
                withDockerRegistry(credentialsId: 'docker-cred', url: '') {
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy to Docker Container') {
            steps {
                sh '''
                docker stop bms-container || true
                docker rm bms-container || true
                docker run -d -p 3000:3000 --name bms-container $DOCKER_IMAGE:latest
                '''
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Build and Deployment Completed",
                body: "The CI/CD pipeline completed successfully.",
                to: "shoton76mm@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "FAILED: CI/CD Pipeline",
                body: "The CI/CD pipeline failed. Check Jenkins for details.",
                to: "shoton76mm@gmail.com"
            )
        }
    }
}
