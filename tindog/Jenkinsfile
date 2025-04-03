pipeline {
    agent any

    environment {
        // Define the image tag with the build number appended to the base name
        buildTag = "tindog-nginx-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'dennis', url: 'https://github.com/TechNgine/docker-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Navigate into the tindog directory and build the Docker image
                    dir('tindog') {
                        sh "docker build -t ${buildTag} ." // Build Docker image with build number appended to the tag
                    }
                    sh 'docker images'  // List all available Docker images
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the container using the image with the build number appended
                    sh "docker run -d --name tindog-container -p 80:80 ${buildTag}"
                    sh 'docker ps -a'  // List all running and stopped containers
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Remove the Docker image
                    sh "docker rmi ${buildTag} || true"  // Remove the image used for the container
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
