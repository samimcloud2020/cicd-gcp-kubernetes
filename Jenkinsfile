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
          stage("Attestation by BINAUTHZ") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                        sh " gcloud beta container binauthz attestations sign-and-create --artifact-url="gcr.io/genuine-fold-316617/cicd@sha256:a629154f5512c1b6da0b293e44ff3474a3c49b88833cdc43f7cc9115271831ba" --attestor="samim" --attestor-project="genuine-fold-316617" --keyversion-project="genuine-fold-316617" --keyversion-location="global" --keyversion-keyring="keyring5" --keyversion-key="key5" --keyversion="1" "
                    
                }
                }
            }
          }
                
}
}
