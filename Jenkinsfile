pipeline {
    environment {
        registry = "registry-medialab.afp.com"
        registryCredential = "Medialab_Docker_Registry"
        version = ""
        dockerImage = ""
        buidImage = ""
    }
    agent any
    
    stages {
    	stage ('Build package') {
            when {
                branch 'main'
            }
            steps {
                sh '${M2_HOME}/bin/mvn -B -DskipTests clean package' 
            }
        }
        stage ('Build Docker Image') {
        	when {
                branch 'main'
            }
        	steps{
	        	script {
	                def version = sh script: '${M2_HOME}/bin/mvn help:evaluate -Dexpression=project.version -q -DforceStdout', returnStdout: true
	                dockerImage = "registry-medialab.afp.com/weverify-wrapper:${version}"    
	                println "build image ${dockerImage}"
	                docker.withRegistry('https://'+registry, registryCredential) {
	                	def buidImage = docker.build("${dockerImage}","-f ./docker/delivery/Dockerfile .")
	                	buidImage.push()
	                	buidImage.push('latest')
	                }           	                          
	        	}
              }                
        }
        stage('Cleaning Up') {
           	when {
                branch 'main'
            }
            steps{
                sh "docker rmi --force $dockerImage"
            }
        }
    }
    post {
        success {
            slackSend channel: 'medialab_builds', message: "Success build & deploy project ${env.JOB_NAME} - ID: ${env.BUILD_ID}", tokenCredentialId: 'medialab_slack_token'
        }
        failure {
            slackSend channel: 'medialab_builds', message: "Error building project ${env.JOB_NAME} - ID: ${env.BUILD_ID}", tokenCredentialId: 'medialab_slack_token'
        }
    }

}