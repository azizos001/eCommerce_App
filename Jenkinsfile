pipeline {
  environment {
    dockerimagename = "7abouba/shopfront"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        script {
          git branch: 'main', url: 'https://github.com/azizos001/eCommerce_App.git'
        }
      }
    }

    stage('Build images') {
      steps {
        script {
          // Build Docker image for the Shopfront service
          dockerImage = docker.build("7abouba/shopfront", "-f shopfront/Dockerfile shopfront")
          
          // Build Docker image for the Product Catalogue service
          docker.build("7abouba/productcatalogue", "-f productcatalogue/Dockerfile productcatalogue")
          
          // Build Docker image for the Stock Manager service
          docker.build("7abouba/stockmanager", "-f stockmanager/Dockerfile stockmanager")
        }
      }
    }

    stage('Pushing images') {
      environment {
        registryCredential = 'dockerhublogin'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            // Push all images to Docker Hub
            dockerImage.push("latest")
            docker.push("7abouba/productcatalogue:latest")
            docker.push("7abouba/stockmanager:latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          // Deploy each service using their respective Kubernetes YAML files
          kubernetesDeploy(configs: "K8S/productcatalogue-service.yaml", kubeconfigId: "kubernetes")
          kubernetesDeploy(configs: "K8S/shopfront-service.yaml", kubeconfigId: "kubernetes")
          kubernetesDeploy(configs: "K8S/stockmanager-service.yaml", kubeconfigId: "kubernetes")
        }
      }
    }
  }
}
