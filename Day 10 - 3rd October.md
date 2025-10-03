## Day 10 - 3rd October

### Lab 1: VPC - Public Private





&nbsp;---------------------------------------------

| VISR -- VPS -> IGW -> Subnet -> Route Table |

&nbsp;---------------------------------------------



VLSM calculator -- 10.0.0.0 /16 250 150



###### -> Go to AWS > VPC > Create VPC 

&nbsp;> Name tag - Custom-VPC

&nbsp;> VPC settings - VPC Only

&nbsp;> IPv4 CIDR - 10.0.0.0/16

&nbsp;> Create VPC



###### -> Go to AWS > Internet gateways > Create internet gateway

&nbsp;> Name tag - my-igw

&nbsp;> Create internet gateway

&nbsp;> select my-igw > Actions > Attach to VPC > Select VPC - Custom-VPC  > Attach internet gateway



###### -> Go to AWS > Subnets > create subnet

&nbsp;> VPC ID - select Custom-VPC

&nbsp;> Subnet 1

&nbsp;  > Subnet name - Public-subnet

&nbsp;  > Availability Zone - us-east-1a

&nbsp;  > IPv4 subnet CIDR block - 10.0.0.0/24

###### &nbsp;> Add New Subnet

&nbsp; > Subnet 2

&nbsp;   > Subnet name - Private-subnet

    > Availability Zone - us-east-1b

    > IPv4 subnet CIDR block - 10.0.1.0/24

&nbsp;> Create subnet





###### -> Go to AWS > Route tables > Create route table

&nbsp;> Name - Public-RT

&nbsp;> VPC  - Custom-VPC

&nbsp;> Create route table

&nbsp;> Select Public-RT

&nbsp; > edit routes > Add route > 0.0.0.0/0 Target - Internet Gateway > Select my-igw > Save changes

&nbsp; > Subnet associations > Edit subnet associations > Select Public Subnet > Save associations



###### -> Create an EC2 instance

&nbsp;> Name - dev-server-public

&nbsp;> Key pair name - custom-vpc

&nbsp;> Network settings > edit

&nbsp; > VPC - (Custom-VPC)

&nbsp; > Subnet - Public-subnet

&nbsp; > Auto-assign public IP - Enable

&nbsp;> Create Security Group

&nbsp; > Name

&nbsp; > SG Rule - HTTP - Anywhere

&nbsp;           - All ICMP - IPv4 - Anywhere



&nbsp;> Launch Instance

&nbsp;> Select dev-server-public > Connect



###### @ dev-server-public @

###### \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



$ ping google.com - This should work

$ ping <private IP> - This should also work



\#yum install httpd -y

\#rpmquery httpd

\# cd /var/www/html/

\#echo "This is my apache server" > index.html

\#yum install httpd -y

\#cd /var/www/html/

\#echo "This is my apache server" > index.html

\#systemctl start httpd

\#systemctl enable httpd

-> Copy Public IP > Paste along with :80



###### -> Go to AWS > NAT gateways > Create NAT gateway // This is to enable private-server to access the internet

&nbsp;> Name - Private-Nat-Gateway

&nbsp;> Subnet - Public-subnet

&nbsp;> Allocate Elastic IP

&nbsp;> Create NAT gateway



###### -> Go to AWS > Route tables > Create route table

&nbsp;> Create Another route table

 > Name - Private-RT

 > VPC  - Custom-VPC

 > Create route table

&nbsp; > Edit routes > Add route > 0.0.0.0/0 Target - NAT Gateway > Select Private-NAT-Gateway > Save changes

  > Subnet associations > Edit subnet associations > Select Private Subnet > Save associations



###### -> Create an EC2 instance

 > Name - database-server-private

 > Key pair name - custom-vpc

 > Network settings > edit

  > VPC - (Custom-VPC)

  > Subnet - Private-subnet

  > Auto-assign public IP - Disable

 > Select Existing Security Group

  

 > Launch Instance

 > Go to dev-server-public terminal

###### ---accessing private through public server:

> copy keypair contents from downloads

\# sudo su -

\# vim custom-vpc.pem

&nbsp;>Paste the contents then :wq

\# chmod 400 custom-vpc.pem

-> Then Connect the database-server-private instace on EC2 page > Copy the ssh and paste in the dev-server-public-terminal

\#ping google.com - This should work



\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_





### Lab 2: INTERNET SHARING USING PUBLIC IP



\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



Go to Ohio Region



VLSM calculator -- 20.0.0.0 /16 250 150



###### -> Go to AWS > VPC > Create VPC

 > Name tag - Custom-VPC-ohio

 > VPC settings - VPC Only

 > IPv4 CIDR - 20.0.0.0/16

 > Create VPC



###### -> Go to AWS > Internet gateways > Create internet gateway

 > Name tag - my-igw-ohio

 > Create internet gateway

 > select my-igw > Actions > Attach to VPC > Select VPC - Custom-VPC-ohio  > Attach internet gateway



###### -> Go to AWS > Subnets > create subnet

 > VPC ID - select Custom-VPC-ohio

 > Subnet 1

   > Subnet name - Public-subnet-ohio

   > Availability Zone - us-east-2a

   > IPv4 subnet CIDR block - 20.0.0.0/24

 > Create subnet



###### -> Go to AWS > Route tables > Create route table

 > Name - Public-RT-ohio

 > VPC  - Custom-VPC-ohio

 > Create route table

 > Select Public-RT-ohio

  > edit routes > Add route > 0.0.0.0/0 Target - Internet Gateway > Select my-igw-ohio > Save changes

  > Subnet associations > Edit subnet associations > Select Public Subnet > Save associations



###### -> Create an EC2 instance

 > Name - prod-server-public

 > Key pair name - custom-vpc-ohio

 > Network settings > edit

  > VPC - (Custom-VPC-ohio)

  > Subnet - Public-subnet-ohio

  > Auto-assign public IP - Enable

 > Create Security Group

  > Name

  > SG Rule - HTTP - Anywhere

            - All ICMP - IPv4 - Anywhere



 > Launch Instance

 > Select prod-server-public > Connect



###### @ prod-server-public @

###### \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



$ ping google.com - This should work

$ ping <private IP> - This should also work



@ dev-server-public @

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-- 1st exit to Public Server from Private Server

-- cat > abc.txt

&nbsp;	HELLO.THIS IS N.VIRGINIA.

-- vim /etc/ssh/sshd\_config

&nbsp;	permitRootLogin yes

&nbsp;	passwordAuthentication yes

-- systemctl start sshd

-- systemctl enable sshd

-- passwd root

-- cd .ssh/

-- ssh-keygen

-- cat id\_rsa.pub -> Copy

&nbsp;THEN IN PROD SERVER

&nbsp;# cd .ssh/

&nbsp;# vim authorized\_keys ->Paste

&nbsp;-- 

-- scp abc.txt root@public ip of ohio machine:/tmp

&nbsp;	secure copy = scp

###### 

###### @ prod-server-public @

###### \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_



-- vim /etc/ssh/sshd\_config

 	permitRootLogin yes

 	passwordAuthentication yes

-- systemctl start sshd

-- systemctl enable sshd

-- cd .ssh/

-- ssh-keygen

-- cat id\_rsa.pub -> Copy

 THEN IN dev-SERVER

 # cd .ssh/

 # vim authorized\_keys ->Paste



-- cd /tmp

-- ll

-- //File should be there

-- passwd root

-- cat > xyz.txt // in root

 	HELLO.THIS IS OHIO

-- scp xyz.txt root@public ip of N.Virginia machine:/tmp





---------------------------------------------------------------------------------



##### LAB 3 - VPC PEERING

Now the file transfer is happening via Internet, lets do it via VPC peering



---------------------------------------------------------------------------------

N.Virgina



left menu -- peering connection -- devops-to-prod -- VPC requester of that region -- same account \& other region -- VPC of Ohio region \& VPC accepter ID (Copy the VPC ID of Ohio machine)





Accept request of peering connection in Ohio machine



N. Virginia:

ping private Ip of Ohio



route table entries in both the regions

N.virginia -- 20.0.0.0/16 -- peering conn -- save 

Ohio -- 10.0.0.0/16 -- peering conn -- save



N.virgnia

ping 20.0.0.88 -- ping happening

cat > new1.txt

scp new1.txt root@prvateipof-ohio:/tmp





Ohio:

ping 10.0.0.82



cat > new2.txt

scp abc.txt root@prvateipof-virginia:/tmp







## !!HOW TO TERMINATE SESSION!!



->Terminate Instances

->NAT gateways > Delete

->Peering connection > Delete

->Eip>Release

->VPC>Delete

->Internet Gateways > Detach























