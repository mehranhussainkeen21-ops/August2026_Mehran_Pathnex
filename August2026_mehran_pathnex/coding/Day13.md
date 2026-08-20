DAY 13 - Advanced Automation with CloudFormation and CI/CD
 Ansible - Multi-Node Nginx SEtup
 - name: setuo nginx on multiple nodes
 hosts: all
 become: yes
 tasks:
 -name: Install Nginx
 yum:
 name: nginx
 state: present

 - name: start nginx service
 service:
 name: nginx
 state: started
 enable: yes
  Terraform - create IAM Users and Policies
  resource "aws_iam_user" "example_user" {
    name = "pathnex_user"
  }

  resource "aws_iam_policy" "example_policy" {
    name    = "pathnex_policy"
    description = "policy for pathnex application"
    policy = jsonencode({
        version = "2012-10-17"
        statement = [
            {
                Action = "s3:ListBucket"
                Effect = allow
                Resource = "arn:aws:s3:::pathnex-bucket"
            },
        ]
    })
  } 
   Kubernetes - Deploy Nginx with ConfigMap ans Secrets
   apiVersion: v1
   kind: configMap
   metadata:
   name: nginx-config
   data:
   nginx.config: |
   server {
    listen 80;
    server_name localhost;
    location / {
        root   /usr/share/ngimx/html;
        index index.html index.html;
    }
   }

   ---
   apiversion: v1
   kind: secret
   metadata:
   name: nginx-secret
   type: opaque
   data:
   password: cGF0aG51eHBhc3M=

   ---
   apiversion: app/v1
   kind: Deployment
   metadata:
   name: pathnex-nginx
   spec:
   replicas: 2
   selector:
   matchlabels:
   app: pathnex-nginx
   template:
   metadata:
   labels:
   app: pathnex-nginx
   spec:
   containers:
   - name: nginx
   image: nginx
   volumeMounts:
   - mountpath: /etc/nginx/nginx.config
   name: nginx-config
   subpath: nginx.config
   volumes:
   - name: nginx-config
   configmap:
   name: nginx-config
   - name: nginx-secret
   secret:
   secretname: nginx-secret
    Jenkinfiles - Multi-Branch Pipeline with Docker Build
    pipiline {
        agent any
        stages {
            stage('build') {
                steps {
                    script {
                        docker.build('pathnex-app')
                    }
                }
            }
            stage('Deploy') {
                steps {
                    script{
                        kubectl apply -f kubernetes/deployment.yaml
                    }
                }
            }
        }
    }
     GitLab CI/CD - Advanced Deployment with Helm
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
    - docker login -u "$cI_REGESTRY_USER" -P "$ci_REGESTRY_PASSWORD"
    - DOCKER PUSH PATHNEX-WEB-APP

    deploy:
    stage: deploy
    script:
    - helm repo add pathnex https://charts.pathnex.com
    - helm install pathnex-nginx pathnex/nginx-ingress



     Docker
     # COPY Vs ADD
     FROM ubuntu
     COPY pathnex-file.txt /opt/pathnex/files/
     ADD pathnex.tar.gz /opt/pathnex/extracted/