pipeline{
agent any;
tools{
maven 'Maven3.6.3'
}
triggers{
//poll SCM
pollSCM('* * * * *')
//BUild Periodically
//cron('* * * * *')
//Github webhook
//githubPush()
}
stages{
	stage('CheckoutCode'){
		steps{
	git branch: 'development', credentialsId: '9fec20c7-2b6d-4061-beb8-28639bc66f5e', 
	url: 'https://github.com/Anithareddy21/maven-web-application.git'
}
}
	stage('Build'){
		steps{
		sh "mvn clean package"
}
}
	stage('ExecuteSonarQubeReport'){
		steps{
		sh "mvn sonar:sonar"
}
}
	stage('UploadArtifactIntoNexus'){
		steps{
		sh "mvn clean deploy"
}
}
	stage('DeployAppToTomcatServer'){
		steps{
		sshagent(['9141fa01-3bb0-4fc4-aaa0-ab7af9e76230']) {
		sh "scp -0 StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.84.194:/opt/apache-tomcat-9.0.54/webapps/"
}
}
}
}//stages closing
post{
	success{
	emailext body: '''Build Over!...

	Regards,
	Nirvanatechnologies''', subject: 'Build Over!...', to: 
	'mylarapuanitha@gmail.com,jagan2220@gmail.com'
	}
	failure{
	}
	always{
	}
}

}//pipeline closing
