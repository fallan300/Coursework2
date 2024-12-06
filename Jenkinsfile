//Jenkinsfile (Dewclarative Pipeline)
pipeline {
         agent any
         environment {
                 DOCKER_HUB_CREDS = credentials('docker')
        }
        stages {
                stage('Docker Image Build') {
                        steps {
                                echo 'Building Docker Image'
                                sh 'docker build --tag fallan300/cw2-server:1.0 .'
                                echo 'Docker Image Built successfully'
                                }
        }
                stage('Test Docker Image') {
                        steps {
                                echo 'Testing Docker iMAGE'
                                sh '''
                                docker image inspect fallan300/cw2-server:1.0
                                docker run --name test-container -p 8081:8080 -d fallan300/cw2-server:1.0
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
                                sh 'docker push fallan300/cw2-server:1.0'
				}
		}

	}
}
