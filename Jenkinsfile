pipeline {

	environment{
		REGISTRY = 	credentials('REGISTRY')
		REGISTRY_HOST = '52.50.166.37'
	
	}
	
	agent any
	stages {
		stage('Docker Registry Log in') {
			steps {
				sh 'docker login ${REGISTRY_HOST} \
					-u ${REGISTRY_USR} -p ${REGISTRY_PSW}'
			}
		}
	}	 
}
