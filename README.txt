1. Install nodejs and npm 

Ubuntu Linux
-------------
sudo apt update
sudo apt install nodejs npm
nodejs --version
npm --version

Windows 
---------
from this link https://nodejs.org/en/download/

1. Run commands
npm install
node app.js

3.Check app 
localhost:3000
localhost:3000/health

4. Build docker image 
docker build -t myweb:latest .

docker images

5. run docker container locally
docker run -it -p 80:3000 --name myweb myweb:latest

6. create AWS ECR private repo named as  myweb-repo and login to it 

aws ecr get-login-password | docker login --username AWS --password-stdin 629170666893.dkr.ecr.us-east-1.amazonaws.com


Linux/MAC
-------------
-Retrieve an authentication token and authenticate your Docker client to your registry.
Use the AWS CLI:
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 629170666893.dkr.ecr.us-east-1.amazonaws.com

Note: If you receive an error using the AWS CLI, make sure that you have the latest version of the AWS CLI and Docker installed.

-Build your Docker image using the following command. For information on building a Docker file from scratch see the instructions here . You can skip this step if your image is already built:
docker build -t myweb-repo .

-After the build completes, tag your image so you can push the image to this repository:
docker tag myweb-repo:latest 629170666893.dkr.ecr.us-east-1.amazonaws.com/myweb-repo:latest

-Run the following command to push this image to your newly created AWS repository:
docker push 629170666893.dkr.ecr.us-east-1.amazonaws.com/myweb-repo:latest

Windows
--------
-Retrieve an authentication token and authenticate your Docker client to your registry.
Use AWS Tools for PowerShell:
(Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin 629170666893.dkr.ecr.us-east-1.amazonaws.com

-Build your Docker image using the following command. For information on building a Docker file from scratch see the instructions here . You can skip this step if your image is already built:
docker build -t myweb-repo .

-After the build completes, tag your image so you can push the image to this repository:
docker tag myweb-repo:latest 629170666893.dkr.ecr.us-east-1.amazonaws.com/myweb-repo:latest

-Run the following command to push this image to your newly created AWS repository:
docker push 629170666893.dkr.ecr.us-east-1.amazonaws.com/myweb-repo:latest

7. Create Networking
VPC
--------
VPC name : myvpc-vpc   10.0.0.0/16

Subnets (4) 2 public subnets and 2 private subnests in 2 different AZ's
------------------------------------------------------------
AZ name : us-east-1a
SUB name: myvpc-subnet-public1-us-east-1a  10.0.1.0/24
SUB name: myvpc-subnet-private1-us-east-1a  10.0.3.0/24

AZ name : us-east-1b
SUB name: myvpc-subnet-public2-us-east-1b  10.0.2.0/24
SUB name: myvpc-subnet-private2-us-east-1b  10.0.4.0/24

Route tables (4)
------------------
myvpc-rtb-public1-us-east-1a
myvpc-rtb-public2-us-east-1b
myvpc-rtb-private1-us-east-1a
myvpc-rtb-private2-us-east-1b

IGW
----
myvpc-igw

NAT GW
---------
SUB name: myvpc-natgw-public1-us-east-1a


8. Configure ECS


Service SG : helloW-3567 shall accept all tcp inbound traffic from ALB SG only 
NAT GW is needed as containers "tasks" needs to download and install npm as per docker file 

