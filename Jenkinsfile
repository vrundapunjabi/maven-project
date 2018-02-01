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
                    echo "Executing junit report"
                    junit '**/target/surefire-reports/*.xml' 
                    echo "Now Archiving Artifacts"
                    archiveArtifacts '**/*.war'
                    echo "Executing Checkstyle Report"
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                }
            }
        }

        stage ('Deploy to Staging'){
            steps{
                timeout(2){
                    input:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-staging'
            }
            post {
                success {
                    echo 'Code deployed to Staging.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}