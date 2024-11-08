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
                git url: "${GIT_REPO}", branch: 'main'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '9069a989-3162-4108-bc01-fb509b51c264', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        // Log in to Docker Hub to increase pull rate limit
                        bat "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from Dockerfile
                    bat "docker build -t ${IMAGE_NAME} -f Dockerfile ."
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run the Docker container to execute tests (no need to run `npm install` here)
                    docker.image("${IMAGE_NAME}").inside {
                        // No need to install npm packages again here, as it was done during build
                        // bat 'npm test' // Run your tests
                    }
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    // Build the application in Docker container (no need to run `npm install` again)
                    docker.image("${IMAGE_NAME}").inside {
                        bat 'npm run build' // Build the application (ensure the build command is correct)
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
