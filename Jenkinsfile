pipeline {

	environment{
		REGISTRY = 	credentials('REGISTRY')
		REGISTRY_HOST = '34.245.33.119'
	
	}
	
	agent any
	stages {
		stage('Docker Registry Log in') {
			steps {
				sh 'docker login ${REGISTRY_HOST} \
					-u ${REGISTRY_USR} -p ${REGISTRY_PSW}'
			}
		}
		
		stage ('Unit test'){
		
			agent{
				docker{
				
					image '${REGISTRY_HOST}/rust-base'
				}
			
			}
			
			steps{
				sh 'rustup default nightly-2018-04-04'
				sh 'cargo test'
			
			}
		
		
		}gi
		
		stage('Docker Compose Up') {
			agent{
			dockerfile{
            filename 'dockerfiles/docker-compose.dockerfile'
            args "-v /var/run/docker.sock:/var/run/docker.sock"
            reuseNode true
			}
			}
			steps {
			sh 'docker-compose up -d'
			sh 'sleep 30'
			}
		}
		stage('Smoke Test') {
			steps {
				//sh 'curl --fail -I http://0.0.0.0:8888/health'
				sh 'docker run --rm --net $(echo ${JOB_NAME} | sed "s@/@_@g")_default byrnedo/alpine-curl --fail -I http://web/health'
				
			}
		}
		stage('Docker Compose Down') {
			agent{
			dockerfile{
            filename 'dockerfiles/docker-compose.dockerfile'
            args "--net host -v /var/run/docker.sock:/var/run/docker.sock"
            reuseNode true
			}
		}
		steps {
        sh 'docker-compose down'
		}
		}
	}	 
}
