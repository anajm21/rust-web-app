pipeline {

	agent any

	stages {
        stage('Primer stage') {
            steps {
				sh 'whoami'
                
            }
        }
		
		stage ('Tiempo'){
		
		agent{
			docker{
				image 'ubunto:latest'
			}
			steps{
				sh 'apt-get update && apt-get install curl -y'
				sh 'curl wttr.in'
			}
		}
	
		}


	}
 }
