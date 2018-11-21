pipeline 
{
    agent any
	
	parameters
	{
		string(name: 'tomcat_dev', defaultValue: '13.233.98.245', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '13.233.154.217', description: 'Production Server')
	}
	
	triggers {
         pollSCM('* * * * *')
     }

	stages
	{
        stage('Build')
		{
            steps {
                sh 'mvn clean package'
				}
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i ~/pemfile/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i ~/pemfile/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}