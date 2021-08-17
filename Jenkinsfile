pipeline{
    //Directives that execute the pipeline 
    agent any
    tools {
        maven 'maven'
    }
    //To retrieve value from pom.xml
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
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

                script{ 

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "MyLabDevOps-SNAPSHOT" : "MyLabDevOps-RELEASE"

                nexusArtifactUploader artifacts:
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', 
                type: 'war']], 
                credentialsId: '86517b35-9777-4f5f-a371-429238e369be', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.54:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}" 
                }              
            }
        }

        //stage 5 : Print some additional information
        stage('Print Environment Variables'){
            steps{
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Name is '${Name}'"
            }
        }

        //stage 4 : Deploying 
        stage('Deploy'){
            steps{
                echo 'deploying'
            }
        }
    
    }
}