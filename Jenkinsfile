pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Aravind011464/DevopsFinal.git'
        IMAGE_NAME = 'my-node-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository
                git url: "${GIT_REPO}", branch: 'main' // Ensure you specify the correct branch, e.g., 'main' or 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run the Docker container to execute tests
                    docker.image("${IMAGE_NAME}").inside {
                        sh 'npm install'  // Ensure dependencies are installed before running tests
                        sh 'npm test'     // Run your tests
                    }
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    // Run the Docker container to build the application
                    docker.image("${IMAGE_NAME}").inside {
                        sh 'npm install'  // Install dependencies before building the app
                        sh 'npm run build' // Build the application
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after build
        }
    }
}
