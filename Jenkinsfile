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
    }
}