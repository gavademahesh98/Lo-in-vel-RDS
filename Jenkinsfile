pipeline{

		agent any
		environment{
		
			DOCKERIMG="maheshg98/login-app-rds:${BUILD_NUMBER}"
		}
		
		stages{
		
			stage('build-img and Push'){
			steps{
			
			
			withCredentials([usernamePassword(
                    credentialsId: 'docker-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
					docker build -t $DOCKERIMG .
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKERIMG
                    '''
			
			
			}
			
			}
			
		
		}
		
		stage('deployment'){
		steps{
			sh """
			docker compose down || true
			docker compose pull
			docker compose up -d
		"""
		}
		
		}
}
}
