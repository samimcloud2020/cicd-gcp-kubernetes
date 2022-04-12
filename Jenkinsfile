#!groovy
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
                    myapp = docker.build("gcr.io/genuine-fold-316617/samimcicd:v1.0.0")
                }
            }
        }
             
       
       stage('Push image to GCR') {
           steps {
               script {
                   withDockerRegistry([credentialsId: "gcr: genuine-fold-316617", url: "https://gcr.io"]) {
                       sh "docker push gcr.io/genuine-fold-316617/samimcicd:v1.0.0"
                   
                   }     
               
          }
        }
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/cicd:latest/cicd:${env.BUILD_ID}/g' deploy.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deploy.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}
