pipeline {
	agent any
	
	stages {
		stage('build') {
			steps {
				sh 'ant if build.xml -v'
			}
		}
	}
}
