pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    BG = "HOme"
    WORKER = "Micro"
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
            bat 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    }
    
    stage('NexusArtifactUploaderJob'){
    	steps{
    		nexusArtifactUploader (
				nexusVersion('nexus3')
				protocol('http')
				nexusUrl('localhost:8081')
				groupId('com.mycompany')
				version('1.2.0')
				repository('maven-releases')
				credentialsId('Nexus')
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