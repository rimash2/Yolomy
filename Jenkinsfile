pipeline {
    agent any
    
    tools {nodejs "nodejs"}
    
   
   environment {     
    DOCKERHUB_CREDENTIALS= credentials('dockerhubcredentials')     
} 
    
    
    stages {
        stage('Start') {
            steps {
                echo 'Build is starting'
            }
        }
    
    stage('Clone github repository') {
            steps {
                git url: 'https://github.com/rimash2/Yolomy', branch: 'master'
     
            }
        }
        
    stage('Confirm docker is available') {
            steps {
                bat '''
                   docker --version
                   '''
            }
        }
        
    stage('Build backend image') {
            steps {
                bat '''
                   cd backend
                   docker build -t mngaruiya/g1-yolo-backend:2.0.3 .
                   '''
            }
        }
        
    stage('Build client image') {
            steps {
                bat '''
                   cd client
                   docker build -t mngaruiya/g1-yolo-client:2.0.3 .
                   '''
            }
        }
        
        
    stage('Login to Docker Hub') {      	
          steps{                       	
	         bat 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'   
        		
            	echo 'Login Completed'      
    }           
}   
        stage('Push images to DockerHub') {
            steps {
                bat '''
                  echo 'Pushing client'
                  docker push mngaruiya/g1-yolo-client:2.0.3
                   
                  echo 'Pushing backend'
                  docker push mngaruiya/g1-yolo-backend:2.0.3 
                  '''
            }
        }
        stage('End') {
            steps {
                echo 'Build is finished'
            }
        }
    }
    post {
		always {
			bat 'docker logout'
		}
	}
}