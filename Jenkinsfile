pipeline {
    agent any
    environment {
        dockerHome = tool name: 'myDocker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        mavenHome = tool name: 'myMaven', type: 'hudson.tasks.Maven$MavenInstallation'
        PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
    }
    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                sh 'mvn --version'
                sh 'docker version'
                echo "Build"
                echo "PATH - ${env.PATH}"
                echo "BUILD_NUMBER - ${env.BUILD_NUMBER}"
                echo "BUILD_ID - ${env.BUILD_ID}"
                echo "JOB_NAME - ${env.JOB_NAME}"
                echo "BUILD_TAG - ${env.BUILD_TAG}"
                echo "BUILD_URL - ${env.BUILD_URL}"
            }
        }
        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Integration Test') {
            steps {
                sh "mvn failsafe:integration-test failsafe:verify"
            }
        }
        stage('Package') {
            steps {
                sh "mvn package -DskipTests"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("soniasebastian/currency-exchange-devops:${env.BUILD_TAG}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
		stage('Test') {
    		steps {
   	    		sh "mvn test -X"
    			}
			}
		stage('Integration Test') {
    		steps {
        		sh "mvn failsafe:integration-test failsafe:verify -X"
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
