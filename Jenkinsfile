pipeline {
    agent any
    stages
    {
        stage('ContDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/intelliqittrainings/maven.git'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to download the code from github', cc: '', from: '', replyTo: '', subject: 'Download Failed', to: 'gitadmin@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh label: '', script: 'mvn package'
                    }
                    catch(Exception e2)
                    {
                        mail bcc: '', body: 'Jenkins is unable to build the code', cc: '', from: '', replyTo: '', subject: 'Build Failed', to: 'developers@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('DeployartfacttoS3')
        {
            steps
            {
                script
                {
                    try
                    {
                        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'kattybkt', excludedFile: '/webapp/target', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '**/webapp/target/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'my_artifact', userMetadata: []
                    }
                    catch(Exception e3)
                    {
                        mail bcc: '', body: 'Jenkins is unable to upload the code', cc: '', from: '', replyTo: '', subject: 'artifact upload Failed', to: 'awsadmin@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContDeployment')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh label: '', script: 'scp /home/ubuntu/.jenkins/workspace/PipelineException/webapp/target/webapp.war ubuntu@172.31.1.16:/var/lib/tomcat8/webapps/testapps.war'
                    }
                    catch(Exception e4)
                    {
                        mail bcc: '', body: 'Jenkins is unable to deploy the artifact in QA server', cc: '', from: '', replyTo: '', subject: 'Deployment Failed', to: 'nrusingh7@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/intelliqittrainings/FunctionalTesting.git'
                        sh label: '', script: 'echo "Testing Passed"'
                    }
                    catch(Exception e5)
                    {
                        mail bcc: '', body: 'Testing failed of the app on QA server', cc: '', from: '', replyTo: '', subject: 'Testing Failed', to: 'abhisek@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContDelivery')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh label: '', script: 'scp /home/ubuntu/.jenkins/workspace/PipelineException/webapp/target/webapp.war ubuntu@172.31.1.16:/var/lib/tomcat8/webapps/testapps.war'

                    }
                    catch(Exception e6)
                    {
                        mail bcc: '', body: 'Unable to deploy into prod server', cc: '', from: '', replyTo: '', subject: 'Delivery failed', to: 'prodmanagr@gmail.com'
                        exit(1)
                    }
                }
            }
        }
    }
}

