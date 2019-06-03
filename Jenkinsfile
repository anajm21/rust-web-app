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
			
				sh 'curl wttr.in'
			}
		}
	
		}


	}
 }
