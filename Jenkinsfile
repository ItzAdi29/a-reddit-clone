pipeline {
    agent any
    tools {
        jdk 'JDK-17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonarscanner'
        APP_NAME = "reddit-clone-pipeline"
        RELEASE = "1.0.0"
	registryCredential = 'ecr:us-east-2:awscreds'
        appRegistry = "test-build"
        // vprofileRegistry = "https://951401132355.dkr.ecr.us-east-2.amazonaws.com"
        // cluster = "vprofile"
        // service = "vprofileappsvc"
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'test_branch', url: 'https://github.com/ItzAdi29/a-reddit-clone.git'
            }
        }
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=TestPrj \
                    -Dsonar.projectKey=TestPrj'''
                }
            }
        }
 //        stage("Quality Gate") {
 //    	    steps {
 //        	timeout(time: 10, unit: 'MINUTES') {
 //            	    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
 //            	    // true = set pipeline to UNSTABLE, false = don't
 //            	    waitForQualityGate abortPipeline: true
 //        	}
 //    	    }
	// }

	    
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }

	stage('Build App Image') {
       	    steps {
                script {
                    dockerImage = docker.build( appRegistry + ":$BUILD_NUMBER")
                }
     	    }
    	}

    	// stage('Upload App Image') {
     //        steps{
     //        script {
     //          docker.withRegistry( vprofileRegistry, registryCredential ) {
     //            dockerImage.push("$BUILD_NUMBER")
     //            dockerImage.push('latest')
     //          }
     //        }
     //      }
     // }
     }    
}
