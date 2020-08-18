pipeline{
    agent any
    environment{
      PATH = "${PATH}:${tool name: 'maven3', type: 'maven'}/bin"
    }
    stages{
        stage('SCM Checkout'){
            steps{
                git url: 'https://github.com/git212/genwebapp',
                branch: 'master',
                credentialsId: 'github'
            }
        }

        stage('Maven Build'){
            steps{
                sh 'mvn clean package'
            }
        }

        stage('Deploy Dev'){
             steps{
                sshagent(['tomcat-dev']) {
                    // stop tomcat
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.43.156 /opt/tomcat8/bin/shutdown.sh"
                    // copy war file to remote tomcat
                    sh "scp target/genwebapp.war ec2-user@172.31.43.156:/opt/tomcat8/webapps/"
                    // start tomcat
                    sh "ssh ec2-user@172.31.43.156 /opt/tomcat8/bin/startup.sh"
                }
            }
        }
    }
    post{
        success{
            mail body: '''Hi Team, The app is Successfully deploye
Thanks,
DevOps Team.
git212''', subject: 'Successfully Deployed', to: 'bharatmahanta05@gmail.com'
        }
    }
}