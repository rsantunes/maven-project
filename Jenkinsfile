pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Deploy to staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }
        
        stage('Deploy to production') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Approve PRODUCTION deployment'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production!'
                }
                failure {
                    echo 'Deployment to Production failed.'
                }
            }
        }
    }
}