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
        stage('UNIT testing'){
            
            steps{
                
                script{
                    bat ' mvn -f /demo-counter-app/pom.xml clean install'
                    sh 'mvn -e test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    bat ' mvn -f /demo-counter-app/pom.xml clean install'
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    bat ' mvn -f /demo-counter-app/pom.xml clean install'
                    sh 'mvn clean install'
                }
            }
        }
    
        }
        
}
