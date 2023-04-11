pipeline {
	tools {
	  maven "MAVEN3"
	  jdk "OracleJDK8"

	}

    agent any

    stages {

      stage ("Fetch Code") {
        steps {
          git branch: "master", url: "https://github.com/PrasadVdm/Jenkins-Ansible.git"
        }
      }

      stage ("Unit test") {
        steps {
        	sh "mvn test"
        }
      }

      stage("build & SonarQube analysis") {
        steps {
          withSonarQubeEnv('sonar') {
                sh 'mvn install sonar:sonar'
                }
        }
      }

      stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }


      stage("Uploading artifact to Nexus") {
      	steps {
      		nexusArtifactUploader(
		        nexusVersion: 'nexus3',
		        protocol: 'http',
		        nexusUrl: '172.31.25.2:8081',
		        groupId: 'STAGE',
		        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
		        repository: 'jenkans-cicd',
		        credentialsId: 'nexuslogin',
		        artifacts: [
		            [artifactId: 'vproapp',
		             classifier: '',
		             file: 'target/vprofile-v2.war',
		             type: 'war']
		        ]
     		)
      	}
      }











    }
}