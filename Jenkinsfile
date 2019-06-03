pipeline {

	agent{
			docker{
				image 'ubuntu:latest'
			}
		}

	stages {
        stage('check curl') {
			when {branch 'master'}
            steps {
				sh 'curl --version'
                
            }
        }
		
		stage ('Tiempo'){
		
			steps{
				sh 'apt-get update && apt-get install curl -y'
				sh 'curl wttr.in'
			}
		}
	
	}
 }
