pipeline{
    //Directives that execute the pipeline 
    agent any
    tools {
        maven 'maven'
    }


    stages {

        //Specify the various stage in the Jenkinsfile
        //stage 1 : Build 
        stage('Build') {
            steps {
                sh 'mvn clean install package'
            }
        }

        //stage 2 Testing
        stage ('Test'){
            steps{
                echo 'Testing'
            }
        }

        //stage 3 : Deploying 
        stage('Deploy'){
            steps{
                echo 'deploying'
            }
        }
    
    }
}