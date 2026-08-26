Day 22 - Multi-Cluster Kubernetes Management
 Ansible - Deploy Nginx Across Multiple Kubernetes Cluster
 - name: Deploy Nginx across multiple Kubernetes cluster
 hosts: localhosts
 tasks:
 - name: Configure kubectl for cluster 1
 command: kubectl --context=us-east config use-context us-east-cluster
 - name: deploy nginx on cluster 1
 command: kubectl apply -f kubernetes/deployment.yaml
 - name: configure kubectl for cluster 2
 command: kubectl --context=us-west config use-context us-west-cluster
 - name: Deploy Nginx on cluster 2
 command: kubectl apply -f kubernetes/deployment.yaml
  Terraform - Multi-Region Kubernetes Cluster with EKS and GKE
  resource "aws_eks_cluster" "pathnex_eks" {
    name    = "pathnex-eks-cluster"
    role_arn = aws_iam_role.eks_cluster_role.arn

    vpc_config {
        subnet_ids = [aws_subnet.pathnex_subnet.id]
    }

    depends_on = [aws_iam_role_policy_attachment.eks_cluster_role_policy]
  }

resource "google_container_cluster" "pathnex_gke" {
    name     = "pathnex-gke-cluster"
    location = "us-central1-a"
install_node_count = 3
node_config {
    mechine_type = "n1-standerd-1"
  }
}
  Kubernetes - Federated Deployment with Multiple Cluster
  apiversion: apps/v1
  kind: Deployment
  metadata:
  name: pathnex-deployment
  spec:
  replicas: 3
  selector:
  matchlabels:
  app: pathnex-app
  template:
  metadata:
  labels:
  app: pathnex-app
  spec:
  containers:
  - name: nginx
  image: nginx
  ports:
  - containerPort: 80
  ---
  apiVersion: federation.k8s.io/v1beta1
  kind: FederatedDeployment
  metadata:
  name: pathnex-federated-deployment
  namespace: defult
  spec:
  template:
  spec:
  replicas: 3
  selector:
  matchLabels:
  app: pathnex-app
  template:
  metadata:
  labels:
  app: pathnex-app
  spec:
  containers:
  - name: nginx
  image: nginx
  ports:
  - containerPort: 80
    Jenkinsfile - multi-cluster Deployment Strategy
    pipeline {
        agent any
        stages {
            stage('deployment of cluster 1') {
                steps {
                    script {
                        sh 'kubectl --context=us-east-cluster apply -f kubernetes/deployment.yaml'
                    }
                }
            }
            stage('deploy to cluster2') {
                steps {
                    script {
                        sh 'kubectl --context=us-west-cluster apply -f kubernetes/deployment.yaml'
                    }
                }
            }
        }
    }
     GitLab CI/CD - Multi-Cluster Deployment Strategy
     stages:
     - deploy

     deploy_cluster_1:
     stage: deploy
     script:
     - kubectl --context=us-east-cluster apply -f kubernetes/deployment.yaml

     deploy_cluster_2:
     stage: deploy
     script:
     - kubectl --context=us-cluster apply -f kubernetes/deployment.yaml



       Docker
       # Compose with mySQL
       version: '3'
       services:
       db:
       image: mySQL:8
       container_name: pathnex-db
       environment:
       MYSQL_ROOT_PASSWORD: root
       web:
       image: nginx
       container_name: pathnex-web