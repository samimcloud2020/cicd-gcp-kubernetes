pipeline {
    agent any
    environment {
        VERSION=2.0.0
        OS=linux  
        ARCH=amd64  
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
                        sh " docker pull samimbsnl/cicd:${env.BUILD_ID}"
                        sh " docker tag samimbsnl/cicd:${env.BUILD_ID} gcr.io/genuine-fold-316617/cicd:${env.BUILD_ID}"
                        sh " curl -fsSL "https://github.com/GoogleCloudPlatform/docker-credential-gcr/releases/download/v${env.VERSION}/docker-credential-gcr_${env.OS}_${env.ARCH}-${env.VERSION}.tar.gz" | tar xz --to-stdout ./docker-credential-gcr > /usr/local/bin/docker-credential-gcr && chmod +x /usr/local/bin/docker-credential-gcr"
                        sh "docker-credential-gcr configure-docker"     
                        sh " docker push gcr.io/genuine-fold-316617/cicd:${env.BUILD_ID}"
                    }
                }
            }
        }        
    }
}
