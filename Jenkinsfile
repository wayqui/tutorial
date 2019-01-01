pipeline {
	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: '3.16.129.203', description: 'Staging Server')
	    string(name: 'tomcat_prod', defaultValue: '18.221.142.253', description: 'Production Server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages{
		stage('Build'){
	    	steps {
	        	sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}

		stage ('Deployments'){
			parallel{
				stage ('Deploy to Staging'){
					steps {
						sh "scp -i /Users/joselobm/Documents/CursoJenkins/AWS/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
					}
				}

				stage ("Deploy to Production"){
					steps {
						sh "scp -i /Users/joselobm/Documents/CursoJenkins/AWS/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
					}
				}
			}
		}
	}
}