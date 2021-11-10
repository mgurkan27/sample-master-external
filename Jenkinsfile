pipeline {
    agent any 
    environment {
        registryCredential = 'mgurkan27'
        imageName = 'mgurkan27/external-v1:20'
        dockerImage = ''
	AWS_DEFAULT_REGION = "us-east-1"
    
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
         
	    
      stage ('Deploy') {
       steps {
        sh '''
        declare -n CLUSTER_NAME=my-cluster
        aws eks --region us-east-1  update-kubeconfig --name my-cluster
       
      }
    }
	    
	    //stage('deploy to k8s') {
             //agent {
                //docker { 
                    //image 'google/cloud-sdk:latest'
                    //args '-e HOME=/tmp'
                    //reuseNode true
                        //}
                    //}
            //steps {
			    //echo 'testing k8 step'
			    //echo 'Get cluster credentials'
				//sh '''
					//gcloud container clusters get-credentials cluster-demotest \
					//--zone us-central1-a --project dtc-102021-u104
					//'''
					//echo 'use kubectl set image to update image for container'
					//echo "$imageName:$BUILD_NUMBER"
				//sh '''
					//kubectl set image deployment/events-external events-external=$imageName:$BUILD_NUMBER --record
					//'''
             //}
        //}    
        stage('Remove local docker image') {
            steps{
			    echo 'will do later'
                sh "docker rm -f $imageName:latest"
                sh "docker rm -f $imageName:$BUILD_NUMBER"
            }
        }
    }
}
