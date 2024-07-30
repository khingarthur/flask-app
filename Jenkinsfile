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
        DOCKERHUB_PAT = "Dockerhub-pat2"
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
		withCredentials([string(credentialsId: "Dockerhub-pat2", variable: "DOCKERHUB_PAT")]) {
            sh "docker login -u khingarthur --password-stdin <<< ${DOCKERHUB_PAT}"
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
