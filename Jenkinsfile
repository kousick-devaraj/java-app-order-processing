pipeline {
	agent any 
	stages {
		stage ('Install Maven'){
			steps {
			sh '''
				echo "Installing maven version 3"
				rm -rf /opt/maven/*
				cd /opt/maven
				wget https://downloads.apache.org/maven/maven-3/3.9.12/binaries/apache-maven-3.9.12-bin.tar.gz
				tar -xzf apache-maven-3.9.12-bin.tar.gz
				/opt/maven/bin/mvn -version
				'''
				}
			}
		stage('Check out code') {
			steps{
				git branch: 'main', url: 'https://github.com/kousick-devaraj/java-app-order-processing.git'
				}
	}

		stage('Build with maven') {
			steps {
				sh '/opt/maven/bin/mvn clean package - Dmaven.test.failure.ignore-true'
		}
	}	
}
post {
		success {
			junit '**/target/surefire-reports/TEST-*.xml'
			archiveArtifacts 'target/*.jar'
}
failure {
	echo "Build failed"
}
}
}
