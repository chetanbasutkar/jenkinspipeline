pipeline {
    agent any

    parameters {
         string(name: 'tomcat-dev', defaultValue: '34.201.173.116', description: 'Staging Server')
         string(name: 'tomcat-prod', defaultValue: '54.173.148.3', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/chetan/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat-dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/chetan/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat-prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}