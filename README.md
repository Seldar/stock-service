# Stock service

This repository consists of a sample php project, docker-compose file and jenkinsfile to build test and deploy a docker stack consisting of php, mysql and nginx containers. 

The stack is configured to be deployed to 3 hosts using docker swarm. All hosts are t2-micro servers from aws ec2 set as a target group for the application load balancer distributing the requests to 3 different nginx containers inside these hosts on port 10001.

Manager Node - Jenkins + Php + Nginx ec2-35-158-163-255.eu-central-1.compute.amazonaws.com
Worker Node 1 - Php + Nginx + Mysql ec2-18-195-215-154.eu-central-1.compute.amazonaws.com
Worker Node 2 - Php + Nginx ec2-18-184-8-198.eu-central-1.compute.amazonaws.com
Application Load Balancer stock-service-2139415234.eu-central-1.elb.amazonaws.com

Jenkins job first builds the php container and runs composer install. after that, one php container is spinned up to run phpunit tests. Once the tests pass, container is removed and deploy starts. First the php container is built and tagged, then pushed to dockerhub (vulukut/stock-service-php) to be pulled in all workers. Then this repository is synched to all worker nodes using rsync to work with the containers spin up there. Finally the stack is deployed with the desired replication counts (Php: 3, Nginx: 3, Mysql: 1) Last step is to give some information about the stack/service/nodes used by docker swarm. 
