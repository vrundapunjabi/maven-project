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
                    echo "This is build stage"
               }
               
        }
        stage('deploy'){
            steps{
                 echo "This is deploy stage"
            }
            
        }
    } 

}