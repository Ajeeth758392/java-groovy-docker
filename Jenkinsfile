      node{ def dockerImageName= 'ajeeth758392/javadedockerapp_$JOB_NAME:$BUILD_NUMBER'
      stage('SCM Checkout'){
         git 'https://github.com/LovesCloud/java-groovy-docker'
      }
      stage('Build'){
         // Get maven home path and build
         def mvnHome =  tool name: 'maven', type: 'maven'   
         sh "${mvnHome}/bin/mvn package -Dmaven.test.skip=true"
      }       
     
     stage ('Test'){
         def mvnHome =  tool name: 'maven', type: 'maven'    
         sh "${mvnHome}/bin/mvn verify; sleep 3"
      }
      
     stage('Build Docker Image'){         
           sh "echo '24081991' | sudo -S docker build -t ${dockerImageName} ."
      }  
   
     stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwdajeeth', variable: 'dockerPWD')]) {
              sh "echo '24081991' | sudo -S docker login -u ajeeth758392 -p ${dockerPWD}"
         }
        sh "echo '24081991' | sudo -S docker push ${dockerImageName}"
      }
      
    stage('Run Docker Image'){
            def dockerContainerName = 'javadockerapp_$JOB_NAME_$BUILD_NUMBER'
            def changingPermission='sudo chmod +x stopscript.sh'
            def scriptRunner='sudo ./stopscript.sh'           
            def dockerRun= "sudo -S docker run -p 8082:8080 -d --name ${dockerContainerName} ${dockerImageName}" 
                  sh "sudo sshpass   ssh -o StrictHostKeyChecking=no -T ubuntu@35.197.135.227" 
                  sh "sudo sshpass   scp -r stopscript.sh ubuntu@35.197.135.227:/home/ajeeth_prabhu" 
                  sh "sudo sshpass   ssh -o StrictHostKeyChecking=no -T ubuntu@34.101.126.233${changingPermission}"
                  sh "sudo sshpass   ssh -o StrictHostKeyChecking=no -T ubuntu@34.101.126.233${scriptRunner}"
                  sh "sudo sshpass   ssh -o StrictHostKeyChecking=no -T ubuntu@34.101.126.233${dockerRun}"
            
            
      
      }     
  }
