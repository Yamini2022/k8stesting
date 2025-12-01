pipeline {
    agent any

    environment {
        // Jenkins credentials ID for Docker Hub (username + password)
        DOCKERHUB = credentials('dockerhub-creds')
        
        // Docker image name: replace 'mydockeruser' with your Docker Hub username
        IMAGE = "rayala1330/demo-app"
    }

    stages {

        stage("Checkout Code") {
            steps {
                // Replace with your GitHub repo URL
                git 'https://github.com/Yamini2022/k8stesting.git'
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t $IMAGE:latest docker/"
            }
        }

        stage("Login to DockerHub") {
            steps {
                // Jenkins injects $DOCKERHUB_USR and $DOCKERHUB_PSW from credentials
                sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
            }
        }

        stage("Push Image") {
            steps {
                sh "docker push $IMAGE:latest"
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                sh "ansible-playbook ansible/ansible.yml"
            }
        }
    }
}
