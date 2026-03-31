pipeline {
    agent any

    environment{
	DOCKER_USER = "nityavadoni"	
	IMAGE_NODE = ${DOCKER_USER}/node-app
	IMAGE_NGINX= ${DOCKER_USER}/nginx-app
    }
	
    stages {
        stage('Image Build') {
            steps {
                sh 'docker build -t node-app .'
		sh 'docker build -t nginx-app .'
            }
        }
	}
        stage('Login to dockerhub') {
            steps {
                withCredentials([usernamePassword(
		    credentialsId: 'dockerhub-creds',
		    usernameVariable: 'DOCKER_USER' ,
		    passwordVarialble: 'DOCKER_PASS'
		)]) {
			sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}
