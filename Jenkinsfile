
    def dockerbuild 


    stage ('Clone repositry') {
                                 
                       checkout scm
         }


    stage('Build image') {

                    app = docker.build("calculator") 
}

      
        stage('test image') {

                    app.inside {
                                 echo "Test Passed"
                           }

                 }
           
          
        stage('Push image') {

                   docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials')
 {
                  app.push("$(env.BUILD_NUMBER)")
                  app.push("latest")  
   }
   echo "pushing docker build to dockerhub"
      


        }
           
          