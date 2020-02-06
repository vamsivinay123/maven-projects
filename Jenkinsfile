pipeline{
	agent any
	
	tools{
		maven 'MAVEN'
	}
	stages{
		stage("BUILD"){
			steps{
				bat label: '', script: 'mvn clean package'
			}
			post{
				success{
					echo "artifact is generated successfully"
					echo "archieving artifact"
					archiveArtifacts '**/*.war'
				}
				failure{
					echo "failed to genearte artifact"
				}
			}
		}
		stage("Deploy to stagging and checking code quality"){
			parallel{
				stage("DEPLOY TO STAGING"){
					steps{
						deploy adapters: [tomcat9(credentialsId: 'a11e1392-50ad-4b6d-9572-85f786ba65bd', path: '', url: 'http://localhost:9091')], contextPath: null, war: '**/*.war'
					}
					post{
					success{
						echo "an application is deployed to staging environment successfully"
					}
					failure{
						echo "failed to deploy an application to staging environment"
					}
					}
				
				}
				stage("CHECKING CODE QUALITY") {
					steps{
						bat label: '', script: 'mvn checkstyle:checkstyle'
						checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
					}
				
				}
			}
			
		}
		
		stage("DEPLOY TO PRODUCTION"){
			steps{
				timeout(time: 4, unit: 'HOURS') {
					input 'Deploy an application to production?'			
				}
				deploy adapters: [tomcat9(credentialsId: 'a11e1392-50ad-4b6d-9572-85f786ba65bd', path: '', url: 'http://localhost:2323')], contextPath: null, war: '**/*.war'
			}
			post{
					success{
						echo "application is deployed to production successfully"
					}
					failure{
						echo "failed to deploy to an  application in production"
					}
				}
		}
	}



}