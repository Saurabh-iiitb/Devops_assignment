pipeline {
  environment {
    registry = "saurabhs23/calculator1"
    registryCredential = 'docker-hub-credentials'
    dockerImage = ''
    dockerImageLatest = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Saurabh-iiitb/Devops_assignment.git'
      }
    }
    
    stage('Building image') {
      steps{
        script {
                 docker.build("calculator") 
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials')
 
                  dockerImage.push("${env.BUILD_NUMBER}")
                  dockerImageLatest.push("latest")  
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
    stage('Execute Rundeck job') {
        steps {
          script {
            step([$class: "RundeckNotifier",
                  includeRundeckLogs: true,
                  jobId: "2125a7f4-cf3c-49b7-ac45-635da518f50b",
                  rundeckInstance: "Rundeck",
                  shouldFailTheBuild: true,
                  shouldWaitForRundeckJob: true,
                  tailLog: true])
          }
        }
       }
    }
  }


