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
		
		
		stage("Build Docker file"){
			
			agent { node 'master' }

		    steps {
			    sh 'docker build -t tranthang2404/simple-java:latest .'      
			}
			
		}
		
		stage("Publish to Docker hub"){
			
			agent { node 'master' }

		    steps {
			    sh 'docker push tranthang2404/simple-java:latest'      
			}
			
		}

	
	
		
		
		stage('Deploy on K8s') {

		  agent { node 'node-k8s' }

		  steps {
			sh 'kubectl apply -f ~/k8s/simple-server-java.yaml'      
		  }
  	    }
    }
}