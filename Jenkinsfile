pipeline {
	agent none
	
	stages {
	
		stage('Unit Test') {
			agent {
				'apache'
			}
			steps {
				sh 'ant -f test.xml -v'
				junit 'reports/result.xml'
			}
		}
		
		stage('build') {
			agent {
				'apache'
			}
			steps {
				sh 'ant -f build.xml -v'
			}
		}
		
		stage('deploy') {
			agent {
				'apache'
			}
			steps {
				sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
			}
		}
		stage('Running on CentOS') {
			agent {
				label 'CentOS'
			}
			steps {
				sh "wget http://jaweed3.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
				sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
			}
		}
	}

	post {
		always {
			archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
		}
	}
}
