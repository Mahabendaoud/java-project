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
                        
                        def pom = readMavenPom file: 'pom.xml'
                        def nexusRepo = pom.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"
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
                            repository: nexusRepo, 
                            version: "${pom.version}"
                    }
                }
            }
        
       
        
        /*stage('Docker Image Build'){
                
                steps{
                    
                    script{
                        
                        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mahabendaoud/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mahabendaoud/$JOB_NAME:latest' 
                       
                     
                    }
                }
            }
         stage('push image to dockerhub'){
                
                steps{
                    
                    script{
                        withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
                            sh 'docker login -u mahabendaoud -p ${dockerhub_passwd}'
                            sh 'docker image push mahabendaoud/$JOB_NAME:v1.$BUILD_ID'
                            sh 'docker image push mahabendaoud/$JOB_NAME:latest' 
                           
                        }
                        
                     
                    }
                }
            }*/
         stage('deploy to tomcat server'){
                
                steps{
                    
                    script{
                        
                       deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://192.168.56.20:8082')], contextPath: 'target/Uber.jar', war: '**/*jar'
                     
                    }
                }
            }
       
           
        }
        
}
