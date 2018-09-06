pipeline {
	agent none
	
	stages {
	
		stage('Unit Test') {
			agent {
				label 'apache'
			}
			steps {
				sh 'ant -f test.xml -v'
				junit 'reports/result.xml'
			}
		}
		
		stage('build') {
			agent {
				label 'apache'
			}
			steps {
				sh 'ant -f build.xml -v'
			}
			
			post {
				success {
					archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
				}
			}
		}
		
		stage('deploy') {
			agent {
				label 'apache'
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
		stage('Test on Debian') {
			agent {
				docker 'openjdk:11-jre'
			}
			steps {
				sh "wget http://jaweed3.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
				sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
			}
		}
		stage('Promote artfacts to green') {
			steps {
				sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
			}
		}
		
	}
}
