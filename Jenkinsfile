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
							docker build --no-cache -t prakhyavanaparthy/capstoneproject:capstoneproject .
						'''
                }

            }
		}
		stage('Push Docker Image') {
           steps {
					withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
						sh '''
							docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
							docker push prakhyavanaparthy/capstoneproject:capstoneproject
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
	}
}

