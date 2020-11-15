pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
				label 'ubuntu-16'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
				label 'ubuntu-16'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
				label 'ubuntu-16'
            }
        }
    }
}