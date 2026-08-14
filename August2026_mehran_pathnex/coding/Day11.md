Day 11 - Install Helm on Server

 Ansible - Install Helm on Server
 - name: Install Helm
 hosts: all
 Become: yes
 tasks:
 - name: Download Helm
get_url:
url: https://get.helm.sh/helm-v3.7.0-linux;amd64.tar.gz
dest: /tmp/helm.tar.gz

- name: extract Helm
unarchive:
src: /tmp/helm.tar.gz
dest: /user/local/bin/
remote_src: yes

- name: set helm binary permissions
file:
path: /user/local/bin/helm
mode: '0755'
 Terraform - provision EC2 with Elastic IP
 resource "aws_eip" "pathnex_eip" {
    instance = aws_instance.pathnex_ec2.id
 }

 resource "aws_instance" "pathnex_ec2" {
    ami      = "ami-0abcd1234abcd1234"
    instance_type = "t3.medium"

tag = {
    name: "pathnex_EC2"
 } 
}
 Kubernetes - Helm cart for Nginx
 helm repo add stable https://charts.helm.sh/stable
 Helm install pathnex-nginx stable/nginx-ingress
  Jenkinsfile - Multi-Stage Pipeline
  pipeline {
    agent any
    environment {
        IMAGE_NAME = 'pathnex-web-app'
    }
  stage {
    stage('checkout') {
        steps {
            git 'https://github.com/pathnex/sample-repo.git'
        }
    }
  stage('build Docker Image'){
    steps {
        script {
             docker.build("${env.IMAGE_NAME}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://docker.io', 'docker-credentials') {
                        docker.image("${env.IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
🔹 GitLab CI/CD — Build, Push, and Deploy with Helm
stages:
  - build
  - push
  - deploy

build:
  stage: build
  script:
    - docker build -t pathnex-web-app .

push:
  stage: push
  script:
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD"
    - docker push pathnex-web-app

deploy:
  stage: deploy
  script:
    - helm repo add stable https://charts.helm.sh/stable
    - helm install pathnex-nginx stable/nginx-ingress



🔹 Docker
# EXPOSE & Port Mapping
FROM nginx
EXPOSE 80
docker run -d -p 8080:80 nginx