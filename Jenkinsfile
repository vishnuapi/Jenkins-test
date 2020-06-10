pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    BG = "HOme"
    WORKER = "Micro"
    
    // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "localhost:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "maven-releases"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "Nexus"
  }
  stages {
    stage('Build') {
      steps {
            bat 'mvn -B -U -e -V clean -DskipTests package'
      }
    }

    stage('Test') {
      steps {
          bat "mvn test"
      }
    }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'jenkins-vv-hello-world-DEV'
      }
      steps {
            bat 'mvn clean package -DskipTests=true'
      }
    }
    
    stage('NexusArtifactUploaderJob'){
    	steps{
    		nexusArtifactUploader (
				nexusVersion: NEXUS_VERSION,
                protocol: NEXUS_PROTOCOL,
                nexusUrl: NEXUS_URL,
                groupId: "com.mycompany",
                version: "1.2.0",
                repository: NEXUS_REPOSITORY,
                credentialsId: NEXUS_CREDENTIAL_ID,
				artifacts: [
					[artifactId :"jenkins-vv-hello-world",
					type:"jar",
					file:"target/jenkins-vv-hello-world-1.0.0-SNAPSHOT-mule-application.jar"]
				]
			);
		}
	}
    
}
}