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
			
			stage("stagging and checkstyle"){
				parallel{
				
					stage("deplpoy to staging"){
						steps{
							deploy adapters: [tomcat9(credentialsId: '71b03af8-273c-4c04-af25-7eb9c64ea835', path: '', url: 'http://localhost:8088')], contextPath: null, war: '**/*.war'
						}
						
						post{
							
							success{
								echo "app sent to stage"
							}
							
							failure{
								echo "app failure to sent to stage "
							}
							
						}
						
					}
					
					stage("check code quality"){
						steps{
							bat label: '', script: 'mvn checkstyle:checkstyle'
							checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
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
			
			
			stage("production"){
				steps{
					
					timeout(time: 5, unit: 'HOURS') {
						input 'deploy app to production??'
					}
					
					deploy adapters: [tomcat9(credentialsId: '71b03af8-273c-4c04-af25-7eb9c64ea835', path: '', url: 'http://localhost:8089')], contextPath: null, war: '**/*.war'
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
			
			
			
			
			
			
		}
	}