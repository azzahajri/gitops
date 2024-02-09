pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = "azzaengineer"
        APP_NAME = "gitops-demo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM') {
            steps {
                git credentialsId: 'github',
                url: 'https://github.com/azzahajri/gitops.git',
                branch: 'master'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps {
                script{
                    docker.withRegistry('', REGISTRY_CREDS ){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker Images'){
            steps {
                sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker rmi ${IMAGE_NAME}:latest"
            }
        }
         
    }    
}
//stage('Build Docker Image') {
 //           steps {
 //               withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'azzahajri8797', usernameVariable: 'azzaengineer')]) {
 //                 sh "docker login -u azzaengineer --password "  
 //               }
 //               
//                sh "docker build -t ${IMAGE_NAME}:latest ."
//            }
//        }
 //   }
