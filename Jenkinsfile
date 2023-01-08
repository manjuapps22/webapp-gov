
pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecp -Dsonar.organization=devsecp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=f407cf51700d5bed6b7ee551995b0eafdbbdc751'
			}
    }	
	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
	}
	  stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://058268435027.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	   
    }	
}
