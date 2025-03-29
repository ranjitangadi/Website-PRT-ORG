pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = "4154928e-471e-4b18-810e-c42ba826b1b0"
    }

    stages {
        stage('Clone Repository') {
            agent { label "slave" }
            steps {
                script { 
                    checkout scm
                }    
            }
        }
        
        stage('Build & Push Docker Image') {
            agent { label "slave" }
            steps {
                script { 
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker build -t ranjitaangadi/proj3 ."
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ranjitaangadi/proj3"
                    }
                }    
            }
        }
        
        stage('Deploy to Kubernetes') {
            agent { label "slave" }
            steps {
                script { 
                    sh "kubectl apply -f deploy.yaml"
                    sh "kubectl rollout restart deployment website-deployment"
                    sh "kubectl apply -f service.yaml"   
                }    
            }
        }
    }
}
