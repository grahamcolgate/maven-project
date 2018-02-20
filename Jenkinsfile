pipeline {
    agent any

    parameters {
         string (name: 'tomcat_dev', defaultValue: '35.177.197.103', description: 'Staging Server')
         string (name: 'tomcat_prod', defaultValue: '35.178.104.123', description: 'Production Server')
    }

    triggers {
         pollSCM ('* * * * *')
     }

stages{
        stage ('Pre Build'){
            steps {
                sh 'mvn --version'
            }
        }
        stage ('Build'){
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
                        sh "scp -i /jenkinsdemo.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /jenkinsdemo.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}

