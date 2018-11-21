pipeline 
{
    agent any

    stages 
	{
			stage('Build') 
			{
				steps {
					sh 'mvn clean package'
					}
			
				post
				{
					success
					{
						echo "Now Archiving"
						archiveArtifacts artifacts : '**/target/*.war'
					}
				}
			}
			stage('Deploy to Staging')
			{
				steps
				{
					build job: 'deploy-to-staging'
				}
				
			}
			stage('Deploy to production') 
			{
				steps {
						timeout(time:5,unit:'DAYS'){
						inputmessage:'Approve Production deployment ?'
					}
					
					buildj job: 'deploy-to-production'
			
			        }
			post{
			success
					{
					echo "Code deployed to production"
					}
				failure{
					echo "Deployed failed."
				}
			
			}
			}
	}
}