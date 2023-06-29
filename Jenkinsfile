pipeline {
    agent any

    tools {
          maven 'localMaven'
          jdk 'localJDK'
    }
    parameters {
         string(name: 'tomcat_staging', defaultValue: '13.232.204.89', description: 'Staging Server')
         //string(name: 'tomcat_prod', defaultValue: '13.57.204.205', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving starts here...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging environment'){
                    steps {
                                        sshagent(credentials: ['26c29081-8ed6-4b51-8da9-31ae6673ab62']) {
                    sh 'ssh root@tomcat-server "sudo systemctl stop tomcat"'
                    sh 'ssh root@tomcat-server "rm -rf /opt/tomcat/webapps/sample"'
                    sh 'scp /opt/tomact/webapps/sample.war root@tomcat-server:/opt/tomcat/webapps'
                    sh 'ssh root@tomcat-server "sudo systemctl start tomcat"

                    }
                }

           //     stage ("Deploy to Production environment"){
           //        steps {
           //             bat "echo y | pscp -i C:\\Users\\grvtr\\Desktop\\Project\\AlternativeFiles\\Redis-Key.ppk C:\\Users\\grvtr\\Desktop\\Project\\AlternativeFiles\\*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
