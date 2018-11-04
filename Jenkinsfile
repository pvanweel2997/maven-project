pipeline {
    agent any
	parameters {
		string(name: 'DEV_DEPLOY',defaultValue:'hello',description:'parameter 1')
	}
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
		stage('Deploy to staging') {
			steps {
				build job: 'deploy-to-staging'
			}
		}
		stage('Deploy to production')  {
			steps {
				timeout(time:5,unit:'DAYS') {
					input message:'Approve production deployment?'
				}
				build job: 'deploy-to-prod'
			}
			post {
				success {
					echo 'Code deployed to production'
				}
				failure {
					echo 'Deployment failed'
				}
			}
		}
		
    }
}