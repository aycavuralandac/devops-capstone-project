pipeline {
    agent any
    stages {
        stage('Linting') {
            steps {
				sh 'tidy -q -e  **/*.html'
			}
        }
        stage('Build blue image') {
            steps {
			    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
				    sh '''
						docker build -t aycav/capstone -f blue/Dockerfile .
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
				withAWS(region:'us-east-1', credentials:'aws-credential') {
					sh '''
						aws eks --region us-east-1 update-kubeconfig --name jenkinscluster
					'''
				}
			}
        }
        stage('Deploy blue container') {
            steps {
				withAWS(region:'us-east-1', credentials:'aws-credential') {
					sh '''
						kubectl apply -f ./blue/blue-controller.json
					'''
				}
			}
        }
        stage('Redirect service to blue container') {
            steps {
				withAWS(region:'us-east-1', credentials:'aws-credential') {
					sh '''
						kubectl apply -f ./blue-green-service.json
					'''
				}
			}
        }
        
     }
}
