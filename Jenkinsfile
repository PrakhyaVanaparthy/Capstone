pipeline{
    agent any
    stages{
        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e app/*.html'
            }
        }
		stage('Build Docker Image') {
           steps {
					withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
						sh '''
							docker build --no-cache -t prakhyavanaparthy/capstoneproject2:latest .
						'''
                }

            }
		}
		stage('Push Docker Image') {
           steps {
					withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
						sh '''
							docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
							docker push prakhyavanaparthy/capstoneproject2:latest
						'''
                }
            }
        }
		stage('Setup Kubectl Context') {
            steps{
					withAWS(region:'us-east-1',credentials:'AWS-Capstone') {
                    sh 'aws eks update-kubeconfig --name capstone'
					sh 'kubectl config use-context arn:aws:eks:us-east-1:148224597888:cluster/capstone'
                }
			}
			}
		stage('Blue Container') {
            	steps{
					withAWS(region:'us-east-1',credentials:'AWS-Capstone') {
					sh 'kubectl apply -f ./blue-container.json'
                }
			}
		}
		stage('Green Container') {
            	steps{
					withAWS(region:'us-east-1',credentials:'AWS-Capstone') {
					sh 'kubectl apply -f ./green-container.json'
                }
			}
		}
		    stage('blue app lb') {
		    steps {
			    withAWS(region:'us-east-1',credentials:'AWS-Capstone'){
				    sh 'kubectl apply -f ./blue-green-service.json'  }  
		    }
		}
	}
}

