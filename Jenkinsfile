pipeline {
    agent any

    environment{
	DOCKER_USER = "nityavadoni"	
	IMAGE_NODE = "${DOCKER_USER}/node-app"
	IMAGE_NGINX= "${DOCKER_USER}/nginx-app"
    }
	
    stages {
        stage('Image Build') {
            steps {
                sh 'docker build -f Dockerfile-Nodejs -t $IMAGE_NODE:latest .'
		        sh 'docker build -f Dockerfile-Nginx -t $IMAGE_NGINX:latest ./Dockerfile-Nginx'
            }
        }
	
        stage('Login to Dockerhub') {
            steps {
                withCredentials([usernamePassword(
		   		 credentialsId: 'dockerhub-creds',
		    	 usernameVariable: 'DOCKER_USER' ,
		    	 passwordVariable: 'DOCKER_PASS'
		)]) {
			sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
            }
        }
		}
        stage('push the images to Dockerhub') {
            steps {
                sh 'docker push $IMAGE_NODE:latest'
				sh 'docker push $IMAGE_NGINX:latest'
            }
        }
    }
}
