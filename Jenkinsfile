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
        stage('Unit test'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration test'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('SonarQube analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
        stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
        stage('Upload artifacts to nexus'){
                
                steps{
                    
                    script{
                        
                        nexusArtifactUploader artifacts: 
                            [
                                [
                                    artifactId: 'springboot', 
                                    classifier: '', file: 'target/Uber.jar', 
                                    type: 'jar'
                                ]
                            ], 
                            credentialsId: 'nexus-auth', 
                            groupId: 'com.example', 
                            nexusUrl: '192.168.56.20:8081', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'demoapp-release', 
                            version: '1.0.0'
                    }
                }
            }
           
        }
        
}
