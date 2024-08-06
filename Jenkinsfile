// scripted


// Declarative

pipeline {
	// agent any
	// agent { docker { image 'maven:3.6.3' } }
	agent { docker { image 'node:13.8' } }


    environment {
        DOCKER_HOST = "unix:///var/run/docker.sock"
    } 
	
	stages {
		stage ('Build') {
			steps {
				sh 'mvn --version'
				echo "Build"
			}
		}
		stage ('Test') {
			steps {
				echo "Test"					
			}	
		}
		stage ('Integration Test') {
			steps {
				echo "Integration Test"
			}	
		}
	} 

	post {
		always {
			echo 'Im awesome. I run always'
		}
		success {
			echo 'I run when you are successful'
		}
		failure {
			echo 'I run when you fail'
		}
	} 
}
		
