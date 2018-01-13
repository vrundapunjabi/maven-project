pipeline {
    agent any
    tools { 
        maven 'maven3' 
        jdk 'java8' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn clean package checkstyle:checkstyle'
            }
            post {
                success {
                    junit '**/target/surefire-reports/*.xml' 
                    archiveArtifacts '**/*.war'
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                }
            }
        }

         stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-maven-project-ude'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'Deploy-to-Prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}