pipeline {
    agent any

    environment {
        name = "firstapp"
        imageName = "flaskapp"
        version = "1.0.${env.BUILD_NUMBER}"
        containerPort = "5000"
        systemPort = "5000"
        registryName = "khingarthur"
        imageUrl = "${registryName}/${imageName}"
        
    }

	
    stages {
        stage("SCM checkout") {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/khingarthur/flask-app.git']])
            }
        }
        stage("list repository content") {
            steps {
                sh "ls -ll"
            }
        } 
        stage("Build the docker image") {
            steps {
                sh "docker build -t $imageName:$version ."
            } 
      }
        stage("Run a container from the image") {
            steps {
               sh "docker run -itd -p $systemPort:$containerPort --name $name $imageName:$version"
            }
        }
        stage("Login into dockerhub") {
            steps {
		withCredentials([secretText(credentialsId: 'Dockerhub-pat2', variable: 'DOCKERHUB_PAT')]) {
            sh "echo $DOCKERHUB_PAT | docker login -u $DOCKERHUB_USR --password-stdin"
		}
       	 }
	}
//        stage("docker tag") {
//            steps {
//                sh " docker tag $imageName $imageUrl:$version"
//            } 
//        }
//        stage("Push the image to dockerhub ") {
//            steps {
//                sh "docker push $imageUrl:$version"
//           }
//        }

    }


}
