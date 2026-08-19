pipeline {
    agent any
    
    environment {
        MAVEN_VERSION = '3.9.16'
        MAVEN_HOME = '/opt/maven/apache-maven-3.9.16'
        // Pro-tip: Since you are adding Maven to the PATH here, you don't actually 
        // need to use the full /opt/maven/... path in your sh scripts below!
        PATH = "/opt/maven/apache-maven-3.9.16/bin:${env.PATH}"
    } // <-- Added the missing closing bracket here

    stages {
        stage ('Install Maven') {
            steps {
                sh '''
                    echo "Installing maven version ${MAVEN_VERSION}"
                    #rm -rf /opt/maven/*
                    #cd /tmp
                    #wget https://dlcdn.apache.org/maven/maven-3/3.9.16/binaries/apache-maven-3.9.16-bin.tar.gz
                    #tar -xzf apache-maven-3.9.16-bin.tar.gz
                    #mv apache-maven-3.9.16 /opt/maven
                    
                    # Fixed the missing space below (mvn -version)
                    /opt/maven/apache-maven-${MAVEN_VERSION}/bin/mvn -version
                    echo "Maven already installed"
                '''
            }
        }
        
        stage('Check out code') {
            steps {
                git branch: 'main', url: 'https://github.com/kousick-devaraj/java-app-order-processing.git'
            }
        }

        stage('Build with maven') {
            steps {
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
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
