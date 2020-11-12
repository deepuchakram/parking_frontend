pipeline {
  environment {
    //registry = "senthil123/parking_ui2"  
    registry = "sudeepthi/parking_ui2" 
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
   agent none
   stages {
      stage('SCM Checkout') {
        agent any
         steps {
                  git 'https://github.com/deepuchakram/parking_frontend.git'         
                }
            }
      stage('Build') {
        agent{
            docker{ image 'node:12-alpine' }
        }
           steps{
              shell 'npm install'
              shell 'npm run build'
                }
            }
      stage('Build Image') {
        agent any
         steps{
          script {
           dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
      stage('Deploy Image') {
        agent any
         steps{
          script {
           docker.withRegistry( '', registryCredential ) {
           dockerImage.push()
      }
     }
    }
   }
      stage('Remove Unused Docker Image') {
         agent any
          steps{
           shell "docker rmi $registry:$BUILD_NUMBER"
               }
          }
      }
 }
