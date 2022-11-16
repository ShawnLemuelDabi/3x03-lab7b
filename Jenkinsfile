pipeline {
	agent none
	stages {
		stage('Integration UI Test') {
			parallel {
				stage('Deploy') {
					agent any
					steps {
						bat 'deploy.bat'
						input message: 'Finished using the web site? (Click "Proceed" to continue)'
						bat 'kill.bat'
					}
				}
				stage('Headless Browser Test') {
					agent {
						docker {
							image 'maven:3-alpine' 
							args '-v /root/.m2:/root/.m2' 
						}
					}
					steps {
						bat 'mvn -B -DskipTests clean package'
						bat 'mvn test'
					}
					post {
						always {
							junit 'target/surefire-reports/*.xml'
						}
					}
				}
			}
		}
	}
}