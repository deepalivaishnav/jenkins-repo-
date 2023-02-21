pipeline {
    agent {
        label{
            label "built-in"
        }
    }
      stages {
        stage('git-clone') {
            steps {
               git 'https://github.com/deepalivaishnav/game-of-life.git' 
            }
        }
        stage('build') {
            steps {
                sh 'mvn install'
            }
        }
        stage('copy-s3') {
            steps {
            s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'deepa-12', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-south-1', showDirectlyInBrowser: false, sourceFile: 'gameoflife-web/target/gameoflife.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'deepa-12', userMetadata: []
        }
        }
        stage('deploy') {
            agent {
                node {
                    label "linux"
                }
            }
            steps {
                sh "aws s3 cp s3://deepa-12/gameoflife.war /mnt"
                sh "sudo docker run -d -p 8085:8080 -v /mnt:/usr/local/tomcat/webapps tomcat:9.0.71"
            }
        }
    }
}
