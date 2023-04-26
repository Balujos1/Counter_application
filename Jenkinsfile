pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/vikash-kumar01/mrdevops_javaapplication.git'
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
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){   //SonarQube Scanner for Jenkins
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            stage('Quality Gate Status'){   //Sonar Quality Gate Plugin
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
            stage('Upload war file to Nexus'){    //Nexus Artifact Uploader
                
                steps{
                    
                    script{
                        
                        nexusArtifactUploader artifacts;
                        artifactsID:
                        credentials:
                        groupId:
                        nexuesUrl:
                        protocol:
                        repository:
                        version:


                    }
                }
            }
         stage('Docker image Building'){

             steps{

              script{
                   
                   sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                   sh 'docker image tag $JOB_NAME:v1.$BUILD_ID vikashashoke/$JOB_NAME:v1.$BUILD_ID'
                   sh 'docker image tag $JOB_NAME:v1.$BUILD_ID vikashashoke/$JOB_NAME:latest'

                }
             }
        }
        stage('Docker image push'){

             steps{

              script{
                   withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
                     
                     sh 'docker login -u vikashashoke -p ${dockerhub_passwd}'
                     sh 'docker image push vikashashoke/$JOB_NAME:v1.$BUILD_ID'
                     sh 'docker image push vikashashoke/$JOB_NAME:latest'
                  }
                }
             }
        }    
        }
        
}
// squ_531616e45ae941db981c0e09f5721d0ca515e4d4  - SonarQube Token ID  (Webhook)
