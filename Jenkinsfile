pipeline {
      environment {
         registry = "yenne1993/springboot-app"
         registryCredential = 'dockerhub'
         dockerImage = ''
  }    
      agent any


      stages { 

         stage('Clone repository') {

                 steps {

                      checkout scm

                    
                    }
                 }
         
         stage('Gradle Build') {

              steps {

                sh './gradlew build'

                }

            } 

         
      stage('Build image') {         
            
             steps {
                
               script {
                   dockerImage = docker.build registry + ":$BUILD_NUMBER"
              
                 docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
              
          
       
                  
         
            } 

         }    
      
     stage('Deploy argocd') {

          steps {
      
              sh "rm -rf argocdmanifest"
              sh "git clone https://github.com/yenne375/argocdmanifest.git"
              sh '''git config user.name "yenne375"'''
              sh "git config --global user.email 'jagadeesh0309@gmail.com'"

           dir("argocdmanifest") {
            sh '''
            git config --global user.email 'jagadeesh0309@gmail.com'
             
            
            cd ./charts/argocd-chart 
            
            echo '$BUILD_NUMBER'            
            yq eval '.image.tag |= "'${BUILD_NUMBER}'"'  values.yaml | tee test403.yaml
            
            cat test403.yaml > values.yaml && rm test403.yaml
            

             cd ../../ && pwd && git commit -am 'Publish new version' && git push git@github.com:yenne375/argocdmanifest.git || echo 'no changes'
              
              '''
          } 

     


   } 


}

}


}




 
       
     
