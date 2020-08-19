pipeline{
    agent any
    environment{
      PATH = "${PATH}:${tool name: 'maven3', type: 'maven'}/bin"
    }
    stages{

        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }

        stage('Deploy - Dev'){
            when {
                branch 'develop'
            }
            steps{
                echo "deploy to dev server"
            } 
        }

        stage('Deploy - UAT'){
            when {
                branch 'staging'
            }
            steps{
                echo "deploy to UAT server"
            } 
        }

        stage('Deploy - prod'){
            when {
                branch 'master'
            }
            steps{
                echo "deploy to prod server"
            } 
        }
    }
}