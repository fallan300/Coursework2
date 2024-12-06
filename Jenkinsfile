//Jenkinsfile (Dewclarative Pipeline)
pipeline {
         agent any
         environment {
                 DOCKERHUB_CREDS = credentials('docker')
		 IMAGE_TAG = "${BUILD_NUMBER}" // Unique tag based on Jenkins build number
        }
        stages {
                stage('Docker Image Build') {
                        steps {
                                echo 'Building Docker Image'
                                sh 'docker build --tag fallan300/cw2-server:${BUILD_NUMBER} .'
                                echo 'Docker Image Built successfully'
                                }
        }
                stage('Test Docker Image') {
                        steps {
                                echo 'Testing Docker iMAGE'
                                sh '''
                                docker image inspect fallan300/cw2-server:${BUILD_NUMBER}
                                docker run --name test-container -p 8081:8080 -d fallan300/cw2-server:${BUILD_NUMBER}
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
                                sh 'docker push fallan300/cw2-server:${BUILD_NUMBER}'
				}
		}
		stage('Deploy') {
                        steps {
                                sshagent(['my-ssh-key']) {
                                        echo 'Hello'
					sh '''
					whoami 
					ssh ubuntu@ec2-3-80-21-186.compute-1.amazonaws.com << EOF
					whoami
					kubectl get pods 
					kubectl set image deployments/my-deployment cw2-server=fallan300/cw2-server:${BUILD_NUMBER} 
					whoami
					<< EOF
					'''
				}
		}

	}
}
}
