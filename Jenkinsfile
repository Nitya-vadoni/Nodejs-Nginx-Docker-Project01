pipeline {
    agent any

    environment{
	DOCKER_USER = "nityavadoni"	
	IMAGE_NODE = "${DOCKER_USER}/node-app"
	IMAGE_NGINX= "${DOCKER_USER}/nginx-app"
	target_vm = "44.200.179.228"
    }
	
    stages {
        stage('Image Build') {
            steps {
                sh 'docker build -f Dockerfile-Nodejs -t $IMAGE_NODE:latest .'
		        sh 'docker build -f Dockerfile-Nginx -t $IMAGE_NGINX:latest .'
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
		stage('Deploy to VM') {
        steps {
        sshagent(['ec2-ssh-key']) {
            sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@<VM-IP> << EOF

            docker pull nityavadoni/node-app:latest
            docker pull nityavadoni/nginx-app:latest

            docker stop node-app || true
            docker stop nginx-app || true

            docker rm node-app || true
            docker rm nginx-app || true

            docker run -d --name node-app -p 3000:3000 nityavadoni/node-app:latest

            docker run -d --name nginx-app -p 80:80 \
            --link node-app:node-app \
            nityavadoni/nginx-app:latest

            EOF
            '''
        }
    }
}
			  
    }
}
