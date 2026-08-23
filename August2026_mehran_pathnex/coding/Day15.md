Day 15 - Ansible with Teraaform, Ansible, and CI/CD Pipelines
 Ansible - Deploy aWeb Application to Multiple Hosts
 - name: Deploy web application to multiple hosts
 host: all
 become: yes
 tasks:
 - name: copy web app files
 copy:
 src: /path/to/web/app/
dest: /var/www/html/
- name: start web app service
service:
name: apache2
start: started
enable: yes
 Terraform - create EC2 with ALB (Application Load Balencer)
resource "aws_lb" "pathnex_lb" {
    name    = "pathnex_lb"
    internal=false
    load_balencer_type = "application"
    security_grop  =[aws_security_group.allow_ssh_http.id]
    subnet = [aws_subnet.public.id]
    }

    resource "aws_instance" "pathnex_ec2" {
        ami             = "ami-0abcd1234abcd1234"
        instance_type   = "t2.micro"
        security_group  = [aws_security_group.allow_ssh_http.name]

        tags            = {
            name        = "Pathnex_EC2"
        }
    }
     Kubernetes - Helm Chart for REdis with Persistence
     helm install pathnex-redis bitnami/redis --set persistence.enable=true --set persistence .size=8Gi
      Jenkinsfile - Multi-Environment Deployment
      pipeline {
        agent any
        environment {
            Image_name = 'pathnex-web-app'
            DEPLOY_ENV = 'production'
        }
        stages {
            stage('build') {
                steps {
                    echo 'building docker image...'
                    docker.build("${env.IMAGE_NAME}")
                }
            }
            stage('test') {
                steps {
                    echo 'running tests...'
                }
            }
            stage('Deploy to Kubernetes') {
                steps {
                    script {
                        if (env.DEPLOY_ENV == 'production') {
                            sh 'kubectl apply -f kubernetes/prod-deployment.yaml'
                        } else {
                            sh 'kubectl apply -f kubernetes/dev-deployment.yaml'
                        }
                    }
                }
            }
        }
      }
       GitLab CI/CD - ENvironment-specific Deployments
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
       - docker login -u "$CI_REGESTRY_USER" -p "$CI_REGISTRY_PASSWORD"
       - docker push pathnex-web-app

       deploy:
       stage: deploy
       script:
       - |
       if ["$CI_COMMIT_REF_NAME"  == "main" ]; then
       kubectl apply -f kubernetes/dev-deployment.yaml
       else
       kubectl apply -f kubernetes/dev-deployment.yaml
       fi


        Docker
        # Multi RUN Commands
        FROM ubuntu
        RUN apt update && \
        apt install curl -y && \
        apt install vim -y
        CMD ["echo", "Hello Pathnex"]