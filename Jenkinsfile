pipeline {
	agent any

    tools {
        maven 'localMaven'
    }

	parameters {
		string(name: 'tomcat_dev', defaultValue: '18.225.31.232', description: 'Staging Server')
	    string(name: 'tomcat_prod', defaultValue: '18.222.222.111', description: 'Production Server')
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