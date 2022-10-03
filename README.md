# CREATE_INSTANCE_USING_AWSCLI

----------------------------------------------AWS INSTANCE PROVISIONING USING AWS CLI --------------------------------------

Creating a VPC

 * aws ec2 create-vpc --cidr-block 172.16.0.0/16

Creating tags for vpc

* aws ec2 create-tags --resources vpc-07032fee4ee7042ec --tags Key=Name,Value=CLI-VPC

Creating Subnets

* aws ec2 create-subnet --vpc-id vpc-07032fee4ee7042ec --cidr-block 172.16.64.0/18  --availability-zone ap-south-1a

Creating tags for subnet

* aws ec2 create-tags --resources subnet-053bb6140310c7cdf --tags Key=Name,Value=CLI-Public-Subnet

Creating an internet gateway

* aws ec2 create-internet-gateway

Creating tags for internet gateway

* aws ec2 create-tags --resources igw-029676ead3e82c3a5 --tags Key=Name,Value=CLI-Internet-Gateway

Attaching an internet gateway to a vpc

* aws ec2 attach-internet-gateway --internet-gateway-id igw-029676ead3e82c3a5 --vpc-id vpc-07032fee4ee7042ec

Creating a Route table

* aws ec2 create-route-table --vpc-id vpc-07032fee4ee7042ec

Creating tags for Route table

* aws ec2 create-tags --resources rtb-0f3262d668e627530 --tags Key=Name,Value=CLI-PUBLIC_RT

Assigning Route table to an Internet gateway

* aws ec2 create-route --route-table-id rtb-0f3262d668e627530 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-029676ead3e82c3a5

Associating subnet to Route table

* aws ec2 associate-route-table --route-table-id rtb-0f3262d668e627530 --subnet-id subnet-053bb6140310c7cdf

Creating security group

* aws ec2  create-security-group --group-name  CLI-WEB-SecurityGroup  --description	"Mysecuritygroup" --vpc-id vpc-07032fee4ee7042ec

Opening 22 port for accessing ssh

* aws ec2 authorize-security-group-ingress --group-id sg-0d6d0cc4e66b1616c --protocol tcp --port 22 --cidr 0.0.0.0/0

Creating a keypair

* aws ec2 create-key-pair --key-name MyKeyPairCLI --query "KeyMaterial" --output text > "C:\AWS\MyKeyPairCLI.pem"

Running the ec2 instance

* aws ec2 run-instances --image-id ami-01216e7612243e0ef --count 1 --instance-type t2.micro --key-name MyKeyPairCLI --security-group-ids sg-0d6d0cc4e66b1616c --subnet-id subnet-053bb6140310c7cdf --associate-public-ip-address

Creating tags for an instance

* aws ec2 create-tags --resources i-0d150a5a31a088747 --tags Key=Name,Value=CLI_EC2

Viewing the instance

* aws ec2 describe-instances

--------------------------Extras-----------------------------------

Viewing the Route Table and Subnets

aws ec2 describe-route-tables --route-table-id <RouteTableId>
  
aws ec2 describe-subnets --filters "Name=vpc-id,Values=<vpcId>"
                         --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"

