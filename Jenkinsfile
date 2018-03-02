pipeline {
    agent any

    parameters {
         string(name: 'tomcat-dev', defaultValue: '35.173.127.132', description: 'Staging Server')
         string(name: 'tomcat-prod', defaultValue: '34.226.141.34', description: 'Production Server')
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
                        sh "scp -i /home/chetan/Desktop/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat-dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/chetan/Desktop/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat-prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}