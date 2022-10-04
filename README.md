# cloassignment1
#run terraform code
tf init/plan/deploy
#clone the repo
git clone https://github.com/bk-c2/cloassignment1
#login to ec2 instance
ssh "ip" -i "key-pair"
#login to ecr repository
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 360201675659.dkr.ecr.us-east-1.amazonaws.com
#pull image from ecr repository staying in ec2 instance
docker pull 360201675659.dkr.ecr.us-east-1.amazonaws.com/clo835assignment1-dev-biratapp:v1.0
docker pull 360201675659.dkr.ecr.us-east-1.amazonaws.com/clo835assignment1-dev-biratdb:v1.0
#run MySQL container
docker run -d --network network1 --name db -e MYSQL_ROOT_PASSWORD=birat123 360201675659.dkr.ecr.us-east-1.amazonaws.com/clo835assignment1-dev-biratdb:v1.0
#run application containers
docker run -d -p 8081:8080 --network network1 --name pinkapp -e APP_COLOR=pink -e DBHOST=172.18.0.2 -e DBPORT=3306 -e DBUSER=root -e DBPWD=birat123 
360201675659.dkr.ecr.us-east-1.amazonaws.com/clo835assignment1-dev-biratapp:v1.0
docker run -d -p 8082:8080 --network network1 --name blueapp -e APP_COLOR=blue -e DBHOST=172.18.0.2 -e DBPORT=3306 -e DBUSER=root -e DBPWD=birat123 
360201675659.dkr.ecr.us-east-1.amazonaws.com/clo835assignment1-dev-biratapp:v1.0 
docker run -d -p 8083:8080 --network network1 --name limeapp -e APP_COLOR=lime -e DBHOST=172.18.0.2 -e DBPORT=3306 -e DBUSER=root -e DBPWD=birat123
360201675659.dkr.ecr.us-east-1.amazonaws.com/clo835assignment1-dev-biratapp:v1.0 
#Execute and run required application container
docker exec -it pinkapp /bin/bash
#verify reachability to other app
ping blueapp or ping limeapp
