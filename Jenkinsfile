pipeline {
    agent any
    stages {
        stage ('Init') {
            steps {
                echo 'Testing ...'
            }
        }

        stage ('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving ...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage ('Deploy to Staging') {
            steps {
                build job: 'Deploy to staging'
            }
        }

        stage ('Deploy to Production') {
            steps {
                timeout(time:5, unit: 'DAYS') {
                    input message: 'Approve PRODUCTION Deployment'
                }
                build job: 'Deploy to prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo 'Deployment Failed...'
                }
            }
        }
    }
}