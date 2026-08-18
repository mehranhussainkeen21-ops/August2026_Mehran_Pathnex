Day 12 - Scaling Applications and Automation
 Ansible - Setup Auto-Scaling Group (ASG)
 - name: Set Up Auto Scaling Group
 hosts: all
 become: yes
 tasks:
 - name: Create Launch Configuration
 ec2_asg:
 name: pathnex-asg
 region: us-east-1
 launch_config_name: pathnex-launch-config
 min_size: 1
 max_size: 3
 desired_capacity: 2
 vpc_zone_identifier: subnet-12345678
  Terraform - Create a Load Balencer with EC2 Instances
  resource "aws_1b" "pathnex_1b" {
    name = "pathnex_1b"
    internal = false
    load_balancer_type = "application"
    Security_groups = [aws_security_group.allow_ssh_http.id]
    subnets = [aws_subnet.public.id]
  }

  resource "aws_instance" "pathnex_ec2" {
    ami  ="ami-0abcd1234abc1234"
    instance_type = "t2.micro"
    security_groups = [aws_security_group._ssh_http.name]
    tags = {
        name = "pathnex-EC2"
    }
  }
   Kubernetes - Horizontal pod Autoscaler (HPA) with Load Balancer
   apiVersion: app/v1
   kind: Deployment
   metadata:
   name: pathnex-web-app
   spec:
   replicas: 2
   selector:
   matchlabels:
   app: pathnex-web-app
   template:
   metadata:
   labels:
   app: pathnex-web-app
   spec:
   containers:
   - name: web
   image: nginx
   ports:
     - cointainerPort: 80
     ---
     apiVersion: autoscaling/v2
     kind: HorizontalPodAutoscaler
     metadata:
     name: pathnex-web-app-hpa
     spec:
     scaleTargetRef:
     apiVersion: apps/v1
     kind: Deployment
     name: pathnex-web-app
     minReplicas: 1
     maxReplicas: 5
     targetCPUUtilizationPercentage: 80
      Jenkinsfile - Implement Parallel Stages
      pipeline {
        agent any
        stages{
            stage ('build & test') {
                parallel {
                    stage('Build') {
                        steps {
                            echo 'building project...'
                        }
                    }
                    stage('test') {
                        steps {
                            echo 'Running tests...'
                        }
                    }
                }
            }
            stage('deploy') {
                steps {
                    echo 'deploying project...'
                }
            }
          }
        }
         GitLab CI/cd - Parellel jobs for Build & Test
         stages:
         - build
         - test
         - deploy

build: 
stage: build
script:
- docker build -t pathnex-web-app .

test:
  stage: test
  script:
  - docker run pathnex-web-app npm test

  deploy:
  stage: deploy
  script:
  - kubectl apply -f kubernetes/deployment.yaml


   Docker
   # Docker Volumes
   FROM ubuntu
   VOLUME ["/opt/pathnex/data"]
   CMD ["SLEEP", "1000"]

   # Real Path
   /opt/pathnex/data

   # Bash commond
   docker volume create pathnex-volume