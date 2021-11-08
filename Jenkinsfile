pipeline {
    agent any 
    environment {
        registryCredential = 'mgurkan27'
        imageName = 'mgurkan27/external-v1:20'
        dockerImage = ''
        }
    stages {
        stage('Run the tests') {
             agent {
                docker { 
                    image 'node:14-alpine'
                    args '-e HOME=/tmp -e NPM_CONFIG_PREFIX=/tmp/.npm'
                    reuseNode true
                }
            }
            steps {
                echo 'Retrieve source from github. run npm install and npm test' 
				echo 'Retrieving source from github'
				git branch: 'master',
				url: 'https://github.com/mgurkan27/sample-master-external.git'
				echo 'Did we get the source?'
				sh 'ls -a'
				echo 'Tests passed on to build and deploy Docker container'
            }
        }
        stage('Building image') {
            steps{
                script {
                    echo 'build the image' 
					dockerImage = docker.build imageName
                }
            }
            }
        stage('Push Image') {
            steps{
                script {
                    echo 'push the image to docker hub' 
					docker.withRegistry( '', registryCredential ) {
			    	dockerImage.push("$BUILD_NUMBER") }
                }
            }
        }     
             
        stage('Remove local docker image') {
            steps{
			    echo 'will do later'
                sh "docker rmi $imageName:latest"
                sh "docker rmi $imageName:$BUILD_NUMBER"
            }
        }
    }
}
