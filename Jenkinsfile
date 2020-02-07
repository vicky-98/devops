pipeline{
	agent any
	
	tools {
		maven 'Maven'
		}
	
		stages{
		
			stage("pacakage"){
				steps{
					bat label: '', script: 'mvn clean package'
				}
				
				post{
					
					success{
						echo "Artifact is generated"
						echo "archive artifact"
						archiveArtifacts '**/*.war'
					}
					
					failure{
						echo "someting went wrong"
					}
					
				}
				
			}
			
			stage("deplpoy to staging"){
				steps{
					build 'maven-webapp-deploy-to-stagging'
				}
				
			}
			
			
			stage("production"){
				steps{
					timeout(time: 5, unit: 'HOURS') {
						input 'deploy app to production??'
					}
					
					build 'maven-webapp-deploy-to-production'
				}
				post{
					
					success{
						echo "App deploy to production success"
					}
					
					failure{
						echo "app deploy to-production failure"
					}
					
				}
				
			}
			
			stage("check code quality"){
				steps{
					build 'maven-webapp-checkstyle'
				}
				
				post{
					
					success{
						echo "app sent to stagging evn"
					}
					
					failure{
						echo "app failure to sent to stagging evn "
					}
					
				}
				
			}
			
			
			
			
		}
	}