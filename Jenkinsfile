pipeline {
    agent any
    stages {
        stage('Linting') {
            steps {
				sh 'tidy -q -e *.html'
			}
        }
        stage('Build blue image') {
            steps {
			    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
				    sh '''
						docker build -t aycav/capstone .
					'''
			    }
		    }
        }        
        stage('Push blue image') {
            steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
						docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
						docker push aycav/capstone
					'''
				}
			}
        }
        stage('create the kubeconfig file') {
            steps {
				withAWS(region:'us-east-1', credentials:'ecr_credentials') {
					sh '''
						aws eks --region us-west-2 update-kubeconfig --name capstone
					'''
				}
			}
        }
        stage('Deploy blue container') {
            steps {
				withAWS(region:'us-west-2', credentials:'ecr_credentials') {
					sh '''
						kubectl apply -f ./blue-controller.json
					'''
				}
			}
        }
        stage('Redirect service to blue container') {
            steps {
				withAWS(region:'us-west-2', credentials:'ecr_credentials') {
					sh '''
						kubectl apply -f ./blue-service.json
					'''
				}
			}
        }
        
     }
}
