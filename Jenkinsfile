// SCRIPTED pipeline does not AUTO Pull from github repo

// DECLARATIVE
pipeline {
	// agent any
	agent { docker { image 'maven:3.6.3'}}
	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
				echo "Build"
			}
		}
		stage('Test') {
			steps {
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Integration Test"
			}
		}
	}
	
	post {
		always {
			echo 'This prints always after every stage of pipeline executes'
		}
		success {
			echo 'This prints only after successful run'
		}
		failure {
			echo 'This prints if run fails'
		}
	}
}
