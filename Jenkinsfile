pipeline {
    agent any

    parameters {
            string(defaultValue: '52.66.187.150', description: 'staging server', name: 'tomcat_dev') 
        }

    triggers {
           pollSCM('* * * * *')
    }

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
                sh "scp -i /var/lib/jenkins/hemali.pem **/target/*.war ubuntu@${params.tomcat_dev}:/root/demo/tomcat-staging/webapps"
            }
        }
    }
}