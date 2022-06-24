pipeline {
    /*
    agent {
        node{
            label 'NBU'
            customWorkspace "workspace/${env.JOB_NAME}"
            }
    }
    */
    agent any	
    environment {
        //GITHUB_TOKEN = credentials('afdcc8c7-083e-4836-b577-3a24ceaca338')
	GITHUB_TOKEN = credentials('nilart-github')    
    }
    options {
        buildDiscarder(logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '5', daysToKeepStr: '30', numToKeepStr: '5'))
        timestamps()
    }
    tools {
        maven 'maven-3.8.5-auto'
        //jdk 'JDK-1.8-new'
    }
    stages {
        stage('Compile') {
            steps {
                sh "echo hi"
		sh "mvn install"
            }
        }
    
    stage('Sonar scan placeholder'){
           steps {
             	sh "echo SonarScan"	
		
           }
	 }
	  
    stage('Build docker image'){
           steps {
             	sh "id"	
		sh "docker build -t nilart/personal-projects:${BUILD_NUMBER} ."
           }
	 }	    
    
    stage('Docker login and push') {
            steps {
              withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
                sh "docker login -u nilart -p ${DockerHubPwd}"
		sh "docker push  nilart/personal-projects:${BUILD_NUMBER}"
              }
            
            }  
         }    
    }
    post {
        always{
            //cleanWorkspace()
	    print "hi"	
        }
        success {
            emailext attachLog: true,
                body: 'Pipeline job ${JOB_NAME} success. Build URL: ${BUILD_URL}',
                recipientProviders: [[$class: 'CulpritsRecipientProvider']],
                subject: 'SUCCESS: Jenkins Job- ${JOB_NAME} Build No- ${BUILD_NUMBER}',
                to: 'nilesh.arte@calsoftinc.com'
        }
        failure {
            emailext attachLog: true,
                body: 'Pipeline job ${JOB_NAME} failed. Build URL: ${BUILD_URL}',
                recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'DevelopersRecipientProvider'], [$class: 'FailingTestSuspectsRecipientProvider'], [$class: 'UpstreamComitterRecipientProvider']],
                subject: 'FAILED: Jenkins Job- ${JOB_NAME} Build No- ${BUILD_NUMBER}',
                to: 'nilesh.arte@calsoftinc.com'
        }
    }
}
