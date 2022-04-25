******************************CICD PIPELINE PYTHON SOURCE CODE**********************************


SOURCE CODE (PYTHON)  ---> REQUIREMENTS.TXT + APP.PY
DOCKERFILE ---> HELP TO MAKE IMAGE
JENKINSFILE ----> STAGE OF BUILD
DEPLOY.YAML ----> GKE DEPLOYMENT & SERVICE WITH LOADBALANCER CREATION

The Jenkisfile hepls to create number of stage
stage 1. To checout code
stage 2 : build image
stage 3: Push image to Dockerhub
stage 4: pull image from dockerhub
stage 5: Tag image which pulled from dockerhub
stage 6: push image to Google Container Registry
stage 7: Deploy to GKE(Google kubernetes engine) Stagging cluster
stage 8: Wait for SRE approval
stage 9: Deploy to GKE(Google kubernetes engine) Production cluster

you have to create to GKE Clusters in GOOGLE CLOUD PLATFORM named stagging & production.
get URL by help of Loadbalancer and you able to access it.
****************************************************************************************************

