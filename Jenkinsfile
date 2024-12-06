//Jenkinsfile (Dewclarative Pipeline)
pipeline {
         agent any
         environment {
                 DOCKERHUB_CREDS = credentials('docker')
		 tag = "${BUILD_NUMBER}" // Jenkins Build ID
        }
        stages {
                stage('Docker Image Build') {
                        steps {
                                echo 'Building Docker Image'
                                sh 'docker build --tag fallan300/cw2-server:${tag} .'
                                echo 'Docker Image Built successfully'
                                }
        }
                stage('Test Docker Image') {
                        steps {
                                echo 'Testing Docker image'
                                sh '''
                                docker image inspect fallan300/cw2-server:${tag}
                                docker run --name test-container -p 8081:8080 -d fallan300/cw2-server:${tag}
                                docker ps
                                docker stop test-container
                                docker rm test-container

                                '''
                }
        }
                stage('Dockerhub Login') {
                        steps {
                                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
				}
		}
                stage('Docker Hub Image Push') {
                        steps {
                                sh 'docker push fallan300/cw2-server:${tag}'
				}
		}
		stage('Deploy') {
                        steps {
                                sshagent(['my-ssh-key']) {
					sh '''
					ssh ubuntu@ec2-3-80-21-186.compute-1.amazonaws.com << EOF
					whoami
					kubectl get pods 
					echo 'setting new image to Kubernetes'
					kubectl set image deployments/my-deployment cw2-server=fallan300/cw2-server:${tag} 
					New image deployed'
					whoami
					<< EOF
					'''
				}
		}

	}
}
}
