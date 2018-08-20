# Stock service

This repository consists of a sample php project, docker-compose file and jenkinsfile to build test and deploy a docker stack consisting of php, mysql and nginx containers. 

The stack is configured to be deployed to 3 hosts using docker swarm. All hosts are t2-micro servers from aws ec2 set as a target group for the application load balancer distributing the requests to 3 different nginx containers inside these hosts on port 10001.

1. **Manager Node** - Jenkins + Php + Nginx ec2-35-158-163-255.eu-central-1.compute.amazonaws.com
2. **Worker Node 1** - Php + Nginx + Mysql ec2-18-195-215-154.eu-central-1.compute.amazonaws.com
3. **Worker Node 2** - Php + Nginx ec2-18-184-8-198.eu-central-1.compute.amazonaws.com
4. **Application Load Balancer** - stock-service-2139415234.eu-central-1.elb.amazonaws.com

Run the jenkins job Stock Service Pipeline to build, test and deploy the stack.
- Jenkins job first builds the php container and runs composer install. 
- One php container is spinned up to run phpunit tests. 
- Once the tests pass, container is removed and deploy starts. 
- First the php container is built and tagged, then pushed to dockerhub (vulukut/stock-service-php) to be pulled in all workers. 
- Then this repository is synched to all worker nodes using rsync to work with the containers spun up there. 
- Finally the stack is deployed with the desired replication counts (Php: 3, Nginx: 3, Mysql: 1) 
- Last step is to give some information about the stack/service/nodes used by docker swarm. 

To access resources use below urls:

- **Jenkins** - [Click here](http://ec2-35-158-163-255.eu-central-1.compute.amazonaws.com:8080/) (user:admin / pass:admin)
- **Stock Service Api** Use this [Postman collection](Service-stock.postman_collection.json) to do requests to the api.

FAQ:

- Why did you decide to use t2.micro servers?

Because the budget is 0€ and amazon offers free tier ec2 instances.
- Why didn't you use a VM instead of spinning up 3 ec2 instances?

Because all VM's except virtualbox is above budget and virtualbox is not allowed to run under Xen which aws uses.
- Why did you use a load balancer?

Because that is one of the ways a swarm stack makes sense. it enables high availability.
- Why is the mysql container deployed to a worker and not master node?

Because t2.micro has limited memory and running jenkins and a mysql container ends up out of memory failures.
- Why did you push the image to dockerhub and not used it locally?

Because workers would not have access to the local images built on master and dockerhub allows a single point of truth.
