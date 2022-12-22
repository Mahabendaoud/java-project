pipeline{
    
    agent any 
    
    tools {
        maven '3.6.3'
    }
    
    
   
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Mahabendaoud/java-project.git'
                }
            }
        }
         stage('Maven build'){
            
            steps{
                
                script{
                    def pomfilepath= <pom.xml /demo-counter-app/prom.xml/ >
                    
                    sh 'mvn clean install'
                }
            }
        }
       stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
       
    
        }
        
}
