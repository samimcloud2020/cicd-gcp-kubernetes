pipeline {
    agent any
    environment {
        VERSION = '2.0.0'
        OS = 'linux'  
        ARCH = 'amd64'  
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
                        
                    }
                }
            }
        }
        stage("Tag image to Dockerhub") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                        sh " docker tag samimbsnl/cicd:${env.BUILD_ID} gcr.io/genuine-fold-316617/cicd:${env.BUILD_ID}"
                        
                    }
                }
            }
        }    
        stage("psuh image to gcr") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                        
                        sh "gcloud iam service-accounts keys create keyfile.json --iam-account jenkinscicd@genuine-fold-316617.iam.gserviceaccount.com"
                        sh " gcloud auth activate-service-account jenkinscicd@genuine-fold-316617.iam.gserviceaccount.com --key-file=keyfile.json"
                        sh " gcloud auth print-access-token | docker login -u oauth2accesstoken --password-stdin https://gcr.io"
                        sh " docker push gcr.io/genuine-fold-316617/cicd:${env.BUILD_ID}"
                    }
                }
            }
        }  
          stage("deploy cloudrun") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                        sh "gcloud run deploy deploy1 --image="gcr.io/genuine-fold-316617/cicd:${env.BUILD_ID}" --platform=managed --region=us-central1 --port=8080 --allow-unauthenticated"
                        sh "gcloud run services add-iam-policy-binding deploy1 --member="allUsers" --role="roles/run.invoker"" 
                }
                }
            
                
}
}
