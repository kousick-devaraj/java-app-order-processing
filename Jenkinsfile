pipeline {
	agent any 
	stages {
		stage ('Install Maven'){
			steps {
			sh '''
				echo "Installing maven version 3"
				rm -rf /opt/maven/*
				cd /tmp
				wget https://dlcdn.apache.org/maven/maven-3/3.9.16/binaries/apache-maven-3.9.16-bin.tar.gz
				tar -xzf apache-maven-3.9.16-bin.tar.gz
				mv apache-maven-3.9.16 /opt/maven
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
