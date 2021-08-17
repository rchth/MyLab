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

        //stage 3 : publish the artefacts to Nexus 

        stage('Publish to Nexus'){
            steps{
nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: '86517b35-9777-4f5f-a371-429238e369be', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.54:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'MyLabDevOps-SNAPSHOT', version: '0.0.4-SNAPSHOT'            }
        }

        //stage 4 : Deploying 
        stage('Deploy'){
            steps{
                echo 'deploying'
            }
        }
    
    }
}