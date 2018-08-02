pipeline {
    agent any

    tools {
	maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev_path', defaultValue: '/opt/apache-tomcat-9.0.10-staging', description: 'Staging Server path')
         string(name: 'tomcat_prod_path', defaultValue: '/opt/apache-tomcat-9.0.10-prod', description: 'Production Server path')
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
                        sh "cp **/target/*.war ${params.tomcat_dev_path}/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war ${params.tomcat_prod_path}/webapps"
                    }
                }
            }
        }
    }
}

