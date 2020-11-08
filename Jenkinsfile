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
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
		stage('Artifacts to S3') { 
           	try {
				withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'deploytos3', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
					sh "aws s3 ls"
					sh "aws s3 mb s3://alexvel.pp.ua"
					sh "aws s3 docker cp 181f08d96d2e:/var/jenkins_home/workspace/simple-java-maven-app/target/my-app-1.0-SNAPSHOT.jar s3://alexvel.pp.ua"
				}
			} catch(err) {
				sh "echo error in sending artifacts to s3"
			}
        }
    }
}