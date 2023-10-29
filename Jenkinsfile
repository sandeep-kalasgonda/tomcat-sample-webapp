pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'jar -cvf sample.war *'
            }
        }
        stage('Deploy') {
            steps {
                sh 'cp sample.war /tomcat/apache-tomcat-10.1.13/webapps/'
            }
        }
    }
	post {
			success {
				// Actions to perform on successful build
				echo 'Build and deployment successful!'
			}
			failure {
				// Actions to perform on build failure
				echo 'Build or deployment failed!'
			}
		}
}

