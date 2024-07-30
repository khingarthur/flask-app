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
        Docker_pat = credentials("Dockerhub-pat2") // Jenkins credentials ID for Docker registry
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
                sh "docker run -itd -p $systemPor:$containerPort --name $name $imageName:$version"
            }
        }
        stage("Login into dockerhub") {
            steps {
                sh "echo $Docker_pat_PSW | docker login -u $Docker_pat_USR --password-stdin"
            }
        }
        stage("docker tag") {
            steps {
                sh " docke tag $imageName $imageUrl:$version"
            } 
        }
        stage("Push the image to dockerhub ") {
            steps {
                sh "docker push $imageUrl:$version"
            }
        }
    }


}
