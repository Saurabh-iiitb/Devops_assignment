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
         
                 dockerImage = docker.build registry + ":$BUILD_NUMBER"
          dockerImageLatest = docker.build registry + ":latest"
         
           
              docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials')
           {
            dockerImage.push()
            dockerImageLatest.push()
          }
           

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
                  jobId: "e059997d-d7fc-421b-ac7e-9f991da8ba9a",
                  rundeckInstance: "Rundeck",
                  shouldFailTheBuild: true,
                  shouldWaitForRundeckJob: true,
                  tailLog: true])
          }
        }
       }
    }
  }


