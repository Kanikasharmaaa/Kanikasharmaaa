pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-kanikasharma0021')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t kanikasharma0021/1.0 .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}


		stage('Push') {

			steps {
				sh 'docker push kanikasharma0021/1.0'
			}
		}


		stage('Deploy to K8s')
		{
			steps{
				sshagent(['k8s-jenkins'])
				{
					sh 'scp -r -o StrictHostKeyChecking=no node-deployment.yaml username@102.10.16.23:/path'
					
					script{
						try{
							sh 'ssh username@102.10.16.23 kubectl apply -f /path/node-deployment.yaml --kubeconfig=/path/kube.yaml'

							}catch(error)
							{

							}
					}
				}
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
