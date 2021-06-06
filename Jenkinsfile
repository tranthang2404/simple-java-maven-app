pipeline {
	agent none                //Not using any `agent` at Global level

      /** Environment Variables**/
      environment {
        registry = "tranthang2404/simple-java"       
        registryCredential = 'docker_hub_cred'               
        dockerImage = ''
      }

  
    stages {
        stage('Build') {
			agent {
				docker {
					image 'maven:3.8.1-adoptopenjdk-11'
					args '-v /root/.m2:/root/.m2'
				}
			}
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') { 
			agent {
				docker {
					image 'maven:3.8.1-adoptopenjdk-11'
					args '-v /root/.m2:/root/.m2'
				}
			}
            steps {
                sh 'mvn test' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
		
		
		stage("version Docker build"){
			
			steps {
				script {
				  dockerImage = docker.build registry + ":latest"           //Build image with tag `latest`
				}
			}

			
		}

	
		stage("Deploy on Docker"){
			
			agent {
				docker {
					image dockerImage
					args '-p 8000:8000'
				}
			}

		
		}
    }
}
