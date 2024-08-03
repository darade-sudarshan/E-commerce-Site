pipeline {
    environment {
        imagename = "daradesudarshan/centralrepo"
        frontendDockerImage = ''
        backendDockerImage = ''
        dockerHubCredentials = 'docker'
        kubernetes_server_private_ip="192.168.49.2"
        ansible_server_private_ip="localhost"
    }
 
    agent any
 
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'git@github.com:darade-sudarshan/E-commerce-Site.git', branch: 'main'])
            }
        }
 
        stage('Building frontend image') {
            steps {
                script {
                    dir('client') {
                        frontendDockerImage = docker.build "${imagename}:PERN-client"
                    }
                }
            }
        }
         stage('Building backend image') {
            steps {
                script {
                    dir('server') {
                        backendDockerImage = docker.build "${imagename}:PERN-server"
                    }
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
                script {
                    // Use Jenkins credentials for Docker Hub login
                    withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
 
                        // Push the image
                        sh "docker push ${imagename}:PERN-client"
                        sh "docker push ${imagename}:PERN-server"
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    // Use Jenkins credentials for Docker Hub login
                    withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
 
                        // Push the image
                        sh "docker run -d -p 8084:3000 ${imagename}:PERN-client"
                        sh "docker run -d -p 8085:9000 ${imagename}:PERN-server"
                    }
                }
            }
        }
    }
    // stage('Copy files from jenkins to kubernetes server'){
     
    // }
 
    // stage('Kubernetes deployment using ansible'){
    
    // }
     // stage('webapp testing'){
    
    // }
}