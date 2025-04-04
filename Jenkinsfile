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
            // Ensure any existing container with the name 'tindog' is removed
            sh 'docker ps -a -q -f name=tindog | xargs -r docker rm -f'
            
            // Build the new Docker image if necessary (this may be skipped if the image already exists)
            sh 'docker build -t tindog-nginx-3 .'
            
            // Run the new container
            sh 'docker run -d --name tindog -p 80:80 tindog-nginx-3'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the container using the image with the build number appended
                    sh 'docker run -d --name tindog -p 80:80 tindog-nginx-3'
                    sh 'docker ps -a'  // List all running and stopped containers
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Remove the Docker image
                    sh 'docker rm -f tindog || true'  // Remove the image used for the container
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
