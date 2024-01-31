//DECLARATIVE
pipeline {
	agent any
	stages { 
		stage('build') {
			steps {
				echo "build"
				echo "test"
				echo "integration test"
			}
		}
		stage('integration test') {
			steps {
				echo "build"
				echo "test"
				echo "integration test"
			}
		}
	} 
	post {
		always { 
			echo 'im awesome . i run always'
		}
		success {
			echo ' i run when you are successful'
		}
		success {
			echo ' i run when you are successful'

	
		}
	}
}