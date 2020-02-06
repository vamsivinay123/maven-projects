pipeline{

		agent any

			tools {
				maven 'MAVEN'
			}

			stages{

				stage("Package"){
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
						echo "something went wrong"
					}
				}
			}


			stage("Deploy to staging"){
				steps{
					build 'maven-webapp-delpoy-to-stagging'
				}
				post{
					success{
						echo "build successfully"	
					}
					failure{
						echo "build failed"
					}
				}
			
			}




		
			stage("Deploy to production"){
				steps{
					timeout(time: 4, unit: 'HOURS')
					{
						input 'Deploy an application to production?'				
					}
					build 'maven-webapp-deploy-to-production'
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