pipeline{
    agent any
    
    tools{
        maven "Maven-3.8.6"
    }
    stages{
        
        stage('Github Clone'){
            steps{
                git branch: 'main', credentialsId: 'my-github-credentials', url: 'https://github.com/OluwasholaPOkoya/java-app.git'
            }
        } 
        
        stage('Build'){
            steps{
                echo "selected environmemt iss : "
                //println params.Environment
                sh "mvn clean install"
            }
       }
       
       stage('Sonar Scan'){
           environment{
               scanner = tool 'Sonar'
           }
           steps{
               withSonarQubeEnv('SonarServer'){
                   sh "${scanner}/bin/sonar-scanner -Dsonar.projectKey=java_app -Dsonar.sources=. -Dsonar.java.binaries=target/classes/com/mycompany/app/"
               }
           }
       }
       
       stage('Deploy'){
           when{
               expression {params.Deploy}
           }
           steps{
               script{
                   if(params.Bucket == "june282022shola"){
                       s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'june282022shola', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/*.jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3', userMetadata: []
                   }
                   if(params.Bucket == "sholabuck"){
                       s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'sholabuck', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/*.jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3', userMetadata: []
                   }
                   if(params.Bucket == "shola-training-bucket"){
                       s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'shola-training-bucket', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-west-1', showDirectlyInBrowser: false, sourceFile: 'target/*.jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3', userMetadata: []
                   }
               }
               
           }
       }
    }
}
