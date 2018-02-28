pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps{
                build job:'deploy-to-staging'
            }
        }
        stage('Deploy to Production'){
            steps{
                timeout(time:5,unit:'Days'){
                    input message:'Approve PRODUCTION Deployment?'
                }
                build job: 'deploy-to-prod'
            }
            post{
                success{
                    echo 'code deployed to Production.'
                }
                failure{
                    echo ' Deployment failed.'
                }
            }
        }
    }
}