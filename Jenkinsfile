pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        // Checkout source code from version control
        checkout scm

        // Build your project (e.g., Maven, Gradle)
        sh 'mvn clean package'
      }
    }

    stage('Test') {
      steps {
        // Run your tests (e.g., JUnit, Selenium)
        sh 'mvn test'
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        // Deploy your application (e.g., Docker, Kubernetes)
        sh sshagent(credentials: ['27b86657-ba75-4b78-9ad6-8a9146bfbb3a']) {
                    sh 'ssh root@tomcat-server "sudo systemctl stop tomcat"'
                    sh 'ssh root@tomcat-server "rm -rf /opt/tomcat/webapps"'
                    sh 'scp /var/lib/jenkins/workspace/SAMPLE/target/webapp/webapp.war root@tomcat-server:/opt/tomcat/webapps'
                    sh 'ssh root@tomcat-server "sudo systemctl start tomcat"'
                    ssh -oStrictHostKeyChecking=no host

      }
    }

    stage('Verify') {
      steps {
         Perform additional verification or integration tests
        sh 'curl http://65.0.26.132:8080/'
      }
    }

    //stage('Deploy to Production') {
      // Run this stage only if the previous stages were successful
      //when {
        //expression {
          //currentBuild.result == 'SUCCESS'
        }
      }

      //steps {
        // Deploy your application to production environment
        //sh 'kubectl apply -f production-deployment.yaml'
      }
   //post {
    //always {
      // Archive your build artifacts
      //archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
   // }
// }
    //success {
      // Send email notification on successful build
      //emailext body: 'The build was successful!', recipientProviders: [[$class: 'DevelopersRecipientProvider']]
   // }

    //failure {
      // Send email notification on failed build
      //emailext body: 'The build failed!', recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    //}
}
