pipeline {
    agent any
    environment {
        PROJECT_ID = 'genuine-fold-316617'
        CLUSTER_NAME = 'cluster1'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'multi-k8s'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("samimbsnl/cicd:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image to Dockerhub") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage("Pull image to Dockerhub") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                            myapp.pll("latest")
                            myapp.pull("${env.BUILD_ID}")
                    }
                }
            }
        }        
    }
