node{
    stage('clone repo'){
        git branch: 'main', url: 'https://github.com/ManjuNK/jenkins-maven-web-app'
    }
    
    stage('maven clean build'){
        def mavenHome = tool name: "Maven-3.8.6", type:"maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    
    stage('code review'){
        withSonarQubeEnv('Sonar-Server-9.4'){
            sh "mvn sonar:sonar"
        }
        
    }
    stage('Upload Artifcate'){
nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'Nexus-Credentails', groupId: 'in.ashokit', nexusUrl: '3.64.237.2:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'manju-snapshot-repository', version: '1.0-SNAPSHOT'    
        
    }
    stage('Deploy'){
        sshagent(['Tomcat-Server_Agent']) {
            sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ubuntu@3.64.237.2:/opt/tomcat/webapps'
        }
    }
}
