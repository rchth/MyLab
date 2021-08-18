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
                file: "target/${ArtifactId}-${Version}.war", 
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

        //stage 6 : Deploying 
        stage('Deploy'){
            steps{
                echo 'deploying'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: 
                    [sshTransfer(
                        cleanRemote: false, 
                        excludes: '', 
                        execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                        execTimeout: 120000, 
                        flatten: false, 
                        makeEmptyDirs: false, 
                        noDefaultExcludes: false, 
                        patternSeparator: '[, ]+', 
                        remoteDirectory: '', 
                        remoteDirectorySDF: false, 
                        removePrefix: '', 
                        sourceFiles: ''
                        )], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }
        }

        stage('Deploy to Docker'){
            steps{
                echo 'deploying'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: 
                    [sshTransfer(
                        cleanRemote: false, 
                        excludes: '', 
                        execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
                        execTimeout: 120000, 
                        flatten: false, 
                        makeEmptyDirs: false, 
                        noDefaultExcludes: false, 
                        patternSeparator: '[, ]+', 
                        remoteDirectory: '', 
                        remoteDirectorySDF: false, 
                        removePrefix: '', 
                        sourceFiles: ''
                        )], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }
        }





    
    }
}