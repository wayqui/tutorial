pipeline {
    agent any

    tools {
        maven 'localMaven'
    }

    stages {
        stage('Build') {
            steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Ahora generando WAR'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
        }

		stage('Deploy to Stagging') {
			steps {
				build job: 'Lesson5-Deploy-to-Stagging'
			}
		}

		stage('Deploy to Production') {
			steps {
				timeout(time:5, unit:'DAYS') {
					input message: 'Aprove PRODUCTION deployment?'
				}
				
				build job: 'Lesson5-Deploy-to-Production'
			}
			
			post {
				success {
					echo 'Despliegue en PRO correcto.'
				}
				
				failure {
					echo 'ERROR en Despliegue en PRO.'
				}
			}
		}
    }
}