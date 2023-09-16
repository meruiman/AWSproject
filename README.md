# AWSproject
First group project requirements:
- [x] Install Wordpress and host on a public subnet, make it highly available. It should scale between 2 to 5 instances based on CPU utilization (>60%) with application load balancer.
- [x] Install MySQL database on a private subnet VM (or you can try RDS)
- [x] Connect Wordpress to database and login to Wordpress in a browser.
Create an instance
* Navigate to AWS console and create VPC “Wordpress”
* After creation VPC we added 2 private and 2 public subnets. We Based in Ohio region and used 2 availability zones.
* Create Internet Gateway and attach to our VPC “Wordpress”
* We created and attached route tables to our subnets.
* Create security group “Wordpress” and open inbound rules SSH 22 port and HTTP 80 port.
* Create security group “MySQL” and edit inbound rules and open port 3306.
* Create EC2 instance “test” and apply code to make sure it works.
* Create launch template “Wordpress” version 1
* Choose Amazon machine “ Amazon Linux 2 AMI”
* Instance type t2.micro
* Proceed without key pair
* Networking settings choose public subnet.
* Choose existing Security group  “Wordpress”
* In Advanced details provide below code
* Create auto scaling group “Wordpress AG”
* Create auto scaling group and name it “Wordpress AG”
* Choose existing “Wordpress TP”
* Choose VPC Wordpress and select all public subnets.
* Attach to a new load balancer (application load balancer) and choose Internet facing scheme. in Group size choose (instances) (desire 2, min 2, max 5)
* In target tracking scaling policy choose average CPU utilization 60% (Target value)
Wordpress bash script :
#!/bin/bash
sudo -i
yum update -y
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum install -y httpd mariadb-server
sudo systemctl start httpd
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
wget https://wordpress.org/latest.tar.gz
tar -xf latest.tar.gz -C /var/www/html/
mv /var/www/html/wordpress/* /var/www/html/
getenforce
setenforce 0
systemctl restart httpd
systemctl enable httpd 
MySQL installation
* Next step is to manually create EC2 instance and name it “MySQL”
* Use same VPC settings.
* Choose private1 subnet. Also edit inbound group.
* Choose existing security group “MYSQL”
* Next step is to create RDS. Choose MySQL 8.0.33 version, Free tier.
* In instance configuration choose db.t3.micro
* On VPC settings choose existing “Wordpress ”
* Public access should be Allow
Connecting the Wordpress
* Navigate to the directory where your WordPress files are located. This is usually under /var/www/html/.
* Edit the wp-config.php using the following lines using the vim editor : 
* define('DB_NAME', 'database_name_here');
* define('DB_USER', 'username_here');
* define('DB_PASSWORD', 'password_here');
* define('DB_HOST', 'localhost');
* 2 Copy your public IP and go to your Wordpress web site
* Complete your Wordpress installation by clicking “Continue”
* Provide your username, password and email
* And click “Install WordPress”
* Log in to your data base , using your username and password


Go to your cloudshell and run the following codes;
mysql -h database-2.cvfcaovaog3m.us-east-2.rds.amazonaws.com -P 3306 -u admin -p
create database 'wordpress'; (wordpress can be any name)
CREATE USER ‘username’@‘%’ IDENTIFIED BY ‘password’; (username can be any name)
GRANT ALL ON wordpress.* TO ‘username’@‘%’;
last command flush privileges
next step is to go to our running instance and connect



Time contrubited by each team member:
1. Azamat - 25
2. Ulan - 25
3. Aidai - 25
4. Meruert - 25
