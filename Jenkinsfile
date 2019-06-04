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
		
		
		}
		
		
		stage('Docker Build') {
			steps {
				sh 'docker build -t ${DOCKER_IMAGE} -f dockerfiles/Dockerfile .'
			}
		}
		
		stage('Docker Up') {
			steps {
        sh 'docker network create --driver=bridge \
            --subnet=172.100.1.0/24 --gateway=172.100.1.1 \
            --ip-range=172.100.1.2/25 ${DOCKER_NETWORK_NAME}'
        sh 'docker run --rm -d --name ${DB_IMAGE} \
            -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} \
            -e MYSQL_DATABASE=${MYSQL_DATABASE} \
            -e MYSQL_USER=${MYSQL_USER} \
            -e MYSQL_PASSWORD=${MYSQL_PASSWORD} \
            --net ${DOCKER_NETWORK_NAME} ${DB_IMAGE}'
        sh 'docker run --rm -d --name ${DOCKER_IMAGE} \
            -e DATABASE_URL=mysql://${MYSQL_USER}:${MYSQL_PASSWORD}@${DB_IMAGE}:3306/${MYSQL_DATABASE} \
            -e ROCKET_ENV=prod \
            --net ${DOCKER_NETWORK_NAME} ${DOCKER_IMAGE}'
        sh 'sleep 30'
			}
		}
		
		/*stage('Docker Compose Up') {
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
		}*/
		/*stage('Smoke Test') {
			steps {
				//sh 'curl --fail -I http://0.0.0.0:8888/health'
				sh 'docker run --rm --net $(echo ${JOB_NAME} | sed "s@/@_@g")_default byrnedo/alpine-curl --fail -I http://web/health'
				
			}
		}*/
		
		stage('Smoke Test') {
			steps {
            sh 'docker run --rm --net ${DOCKER_NETWORK_NAME} \
            byrnedo/alpine-curl --fail -I http://${DOCKER_IMAGE}/health'
			}
		}
	}
	post {
		always {
        sh 'docker kill ${DOCKER_IMAGE} ${DB_IMAGE} || true'
        sh 'docker network rm ${DOCKER_NETWORK_NAME} || true'
		}
	}
		/*stage('Docker Compose Down') {
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
		}*/
		 
}
