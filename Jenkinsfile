pipeline {
    agent any

    environment {
        // Define the image tag with the build number appended to the base name
        buildTag = "tindog-nginx-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/sheff07/docker-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Navigate into the tindog directory and build the Docker image
                    dir('tindog') {
                        sh 'docker ps -a -q -f name=tindog-container | xargs -r docker rm -f'
                        sh 'docker run -d --name tindog-container -p 80:80 tindog-nginx-3' // Build Docker image with build number appended to the tag
                    }
                    sh 'docker images'  // List all available Docker images
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the container using the image with the build number appended
                    sh 'docker run -d --name tindog-container -p 80:80 tindog-nginx-3'
                    sh 'docker ps -a'  // List all running and stopped containers
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Remove the Docker image
                    sh 'docker rm -f tindog-container || true'  // Remove the image used for the container
                    sh 'docker images'  // List available images to confirm deletion

                    // Clean up Jenkins workspace
                    deleteDir()  // Clear the Jenkins workspace
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment and Cleanup Successful! The container is still running.'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
