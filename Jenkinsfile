pipeline {
    agent any
    tools{
        jdk 'java'
        maven 'maven'
    }   
    stages {
        stage('init'){
               steps{
                  sh 'mvn clean package checkstyle:checkstyle'
               }
              post{
                  success{
                      echo "run checkstle report"
                      checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                      echo "unit test checkstyle"
                      junit '**/surefire-reports/*.xml'
                      echo "archieve artifact"
                      archiveArtifacts '**/*.war'

                  }
              }
                
        }
        stage('build'){
               steps{
                    echo "this is build stage"
               }
               
        }
        stage('deploy'){
            steps{
                 build 'deploy-to-stagging'
            }
            
        }
         stage('deploy production'){
            steps{
                 timeout(2) {
                // some block
}
                
            }
    } 
    }
}