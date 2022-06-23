pipeline{
    
    agent{
        label "slave1aws"
    }
    
     
    stages{
        stage('scm'){
            steps{
              
            git 'https://github.com/ratanshakya/jenkins-java-webapp-CI-CD.git'
            }
        }
        stage('Build by Maven Package'){
            steps{
                  sh 'mvn clean package'
              
            }
        }
        
        
         stage('Build Docker Own Image'){
            steps{
                
              sh 'sudo docker build -t ratan123shakya/javawebapp:${BUILD_TAG} .'
              
            }
        }
        
        
        stage('Push Docker Image on DockerHub'){
            steps{
           withCredentials([string(credentialsId: 'DOCKER_HUB', variable: 'my_pass')]) {
    // some block
                sh "sudo docker login -u ratan123shakya -p ${my_pass}"
}
                  sh "sudo docker push ratan123shakya/javawebapp:${BUILD_TAG}"
                 
            }
                
              
        }
        
        stage('Deploy WebApp in Dev Env'){
            steps{
                 sh 'sudo docker rm -f myjavaapp'
                 
              sh "sudo docker run -d -p 8080:8080 --name myjavaapp ratan123shakya/javawebapp:${BUILD_TAG}"
              
            }
        }
        
         stage('Deploy WebApp on Q/A_Env'){
            steps{
                sshagent(['Q/A Team']){
                 
                 sh "ssh -o StrictHostKeyChecking=no ec2-user@13.233.148.23 sudo docker rm -f myjavaapp"
              sh "ssh ec2-user@13.233.148.23 sudo docker run -d -p 8080:8080 --name myjavaapp ratan123shakya/javawebapp:${BUILD_TAG}"
    
                }
            }
        }
        
        
       stage('approved') {
            steps {
                
            
            script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                }
            
                
            }
        }
        }
        
        
        
        
        
         stage('Deploy WebApp on prod_Env'){
            steps{
                sshagent(['Q/A Team']){
                 
                 sh "ssh -o StrictHostKeyChecking=no ec2-user@13.234.66.17 sudo docker rm -f myjavaapp"
                 
                 sh "ssh ec2-user@13.234.66.17 sudo docker run -d -p 8080:8080 --name container ratan123shakya/javawebapp:${BUILD_TAG}"
    
                }
            }
        }
    
        
}
}
