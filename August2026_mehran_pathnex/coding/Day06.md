# Day 06 - Intruduction to Cloudformation and Kubernetes advance CONCEPTS

##  Ansible - Install Docker

```yaml
- name: Install Docker
host: all
become: yes
tasks: 
- name: Install docker
yum:
name: Docker
state: present
- name: start docker
service:
name: Dockker
state: started
enabled: yes


 Terraform - Provisioning EC2 with VPC
resource "aws-vpc" "main" {
cidr-block = "10.0.0.0/16"
}

resource "aws-instance" "Pathnex-EC2" {
ami  ="ami-0abcd1234abcd1234"
instance_type  ="t3.medium"
subnet_id  = aws_subnet.medium.id
tag  = {
Name  = "Pathnex-EC2"
   }
 }
 Kubernetes - pod with Volume Mount
 apiversion: v1
 kind: pod
 metadata:
 name: pathnex-pod
 spec:
 containers:
 - name: web
 image: nginx
 volumemounts:
 - mountPath: /user/share/nginx/html
 name: html-volume
 volumes:
 - name: html-volume
 hostpath:
 path: /data
 type: Directory
  Shell Script - Check for Running Services
  #!/bin/bash
  services= ("nginx" "docker" "httpd")
  for service in "${services[@]}"
  do
  if systemctl is-active --quiet $service; then
  echo "Service is renning"
  else
echo "$service is not running"
fi
done


 Docker
 # node.js Application (javascript)
 console.log("hello pathnex");

 # Docker File
 FROM node:18
 WORKDIR /opt/pathnex/node-app
 COPY app.js /opt/pathnex/node-app/
 CMD ["node", "/opt/pathnex/node-app/app.js"]

 # real path
 /opt/pathnex/mode-app