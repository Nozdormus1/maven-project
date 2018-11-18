pipeline {
    agent any

    tools {
        maven: 'localMaven'
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }

            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Stage'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage('Deploy to production'){
           steps {
               timeout(time:5, unit: 'DYAS'){
                   input_message: 'Approve production deployment?'
               }

               build job: 'deploy-to-prod'
           }

           post {
               success {
                   echo 'Code deployed to Production.'
               }

               failure {
                   echo 'Deployment failed.'
               }
           }
        }
    }
}
