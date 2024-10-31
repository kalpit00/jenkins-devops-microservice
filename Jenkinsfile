// SCRIPTED pipeline does not AUTO Pull from github repo
// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// 	stage('Integration Test') {
// 		echo "Integration Test"
// 	}
// }


// DECLARATIVE
pipeline {
	agent any
	stages {
		stage {
			steps('Build') {
				echo "Build"
			}
		}
		stage {
			steps('Test') {
				echo "Test"
			}
		}
		stage {
			steps('Integration Test') {
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
