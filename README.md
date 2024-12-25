# Accessing-AWS-S3-buckets-remotely---Hands-on
Accessing AWS S3 buckets remotely - Hands-on

Project Status: Concept – Minimal or no implementation has been done yet, or the repository is only intended to be a limited example, demo, or proof-of-concept.

I'm currently preparing AWS Certified Solution Architect - Associate certificate.
The following gist is intended to AWS users to learn more about Amazon Elastic Compute Cloud.
I performed this setup on my Ubuntu 18.04.2 LTS.
To check your OS version, execute $ lsb_release -a in your Terminal.
Installations

None. Just log into your AWS management console, https://console.aws.amazon.com.
You'll need to perform several tasks in your CLI regarding SSH keygen, so make sure you check the following prerequisites.
Prerequisites

First, make sure Oracle jdk is installed. I recommend java 1.8.0
To uninstall effectively your current jdk, perform this:
$ sudo apt-get remove openjdk*
$ sudo apt-get remove --auto-remove openjdk*
$ sudo apt-get purge openjdk*
$ sudo apt-get purge --auto-remove openjdk*

To install java 1.8.0, open Terminal Ctrl+Alt+T and run the command:
$  sudo add-apt-repository ppa:webupd8team/java // adds PPA repository
$  sudo apt-get update // updates package list
$  sudo apt-get install openjdk-8-jdk // installs openjdk

java-8.png
$  javac -version // shows your new java version
Author

    Isaac Arnault - Suggesting installations of major Big Data tools.

IAM_EC2_S3.md
Part 1 : create a User and a Group using IAM

    Log into your AWS management console using $ https://console.aws.amazon.com.

I'm using MFA to secure my root account access coupled with Google Authenticator on my Android smartphone.

You can bypass this step and login normally.
🔴 See output

isaac-arnault-AWS-1.jpg

    Go to Services > IAM > Users > Add user

User name : user-1

Access type : Programmatic access

🔴 See output

isaac-arnault-AWS-16.png

Next : Permissions > Create group
Group name : Developers

Administrator Access > Create group
🔴 See output

isaac-arnault-AWS-17.png

Next : Tags
Key: dev-1 | Value: name of the developer

Create user
🔴 See output

isaac-arnault-AWS-18.png

Download .csv (you gonna use these credentials later on this tutorial)

    Write down your Access key ID and Secret access key > close the window

🔴 See output

isaac-arnault-AWS-28.png

    Now in Groups you should have one group named Developers which should list user-1.

🔴 See output

isaac-arnault-AWS-20.png

Part 2 : create an EC2 instance

Services > EC2

    In "Create Instance" section, click on "Launch Instance"

🔴 See output

isaac-arnault-AWS2.png

    Select Amazon Linux 2 AMI (HVM), SSD Volume Type

    Instance type: choose t2.micro (Free tier eligible). Instance comes with 1vCPU and 1 GiB (memory).

Next: Configure instance details

    Leave all fields as they're by default, just Enable termination protection.

🔴 See output

isaac-arnault-AWS3.png

Next : Add Storage

    Leave default configuration then click on Next: Add Tags. You can leave tags blanks, here I'm using some tags for my own needs.

🔴 See output

isaac-arnault-AWS4.png

Next : Configure Security Group

    Create a new security group > Security group name: dev-group > Description : Developers Security Group > Review and launch > Launch > Create New Key Pair > Key Pair Name : EC2KP > Download Key Pair.

🔴 See output

isaac-arnault-AWS-21.png

Launch Instances > View Instances

    Rename your instance to "DEV".

🔴 See output

isaac-arnault-AWS-22.png

    At this point of the tutorial, you should have one running EC2 instance, a User and a Group via your IAM.

Part 3 : use your Command Line Interface to connect to your S3 buckets

Remember that you've downloaded EC2KP.pem file (go to your Downloads). You will now move that file to a new directory.

Ctrl + Alt + T to open a new CLI window

$ cd Desktop > $ mkdir SSH

$ cd Downloads > $ sudo mv /home/zaki/Downloads/EC2KP.pem /home/zaki/Desktop>SSH

    Go to your SSH directory and check that the file persists there : $ cd Desktop/SSH > ls

    Change the permissions to .pem file, ie: $ chmod 400 EC2KP.pem.

🔴 See output

isaac-arnault-AWS-23.png

Connect to your EC2 instance using your CLI

Use : $ ssh ec2-user@your-ipv4-public-address -i EC2KP.pem.

Type "yes" when prompted by the CLI
🔴 See output

isaac-arnault-AWS-24.png

    Go in root mode : $ sudo su and use $ aws s3 ls. The last command should return "Unable to locate credentials. You can configure credentials by running "aws configure".

To use your provided credentials use : $ aws configure

Remember that you wrote down your Access Key ID and Secret access key when creating your EC2 Instance. Use the provided credentials (go to your Downloads and check for the credentials.csv file).

    Provide Access Key ID > AWS Secret Access Key > Default region name (use the Availability Zone of your EC2 instance, ie : us-east-1) > default output format : you can use "text" or "json". In this tutorial i'm using "json".

$ aws s3 ls displays my available buckets. If your buckets do not show up, go to Users > Security credentials > Create a new access key or create a new EC2 instance and restart the procedure in your CLI.

$ aws s3 ls - Outputs that I have 4 available buckets.
🔴 See output

isaac-arnault-AWS-25.png

Part 4 : create and provision new buckets on S3

$ aws s3 mb s3://nameofyourbucket - To create a new bucket.
🔴 See output

isaac-arnault-AWS-28.png

$ aws s3api put-object --bucket bucketname --key foldername/ - To create a new folder in a specific bucket.

$ aws s3 ls myfifthbucket - To check what's in your bucket.

$ aws s3 cp /filepath/filename s3://bucketname/foldername/ - To upload a file in your bucket.
🔴 See output

isaac-arnault-AWS-29.png

AWS CLI : other useful commands

$ aws s3 rm s3://bucketname/foldername/filename - To delete a specific file in a bucket.

$ aws s3 rm s3://bucketname/ --recursive --exclude "*.jpg" - Recursively deletes all objects under a specified bucket and prefix when passed with the parameter --recursive while excluding some objects by using an --exclude parameter.

$ aws s3 rb s3://bucketname - To delete a bucket.

$ aws s3 rb s3://bucketname --force - To force bucket deletion.

$ aws s3 ls s3://bucketname/foldername/ - To check what's in a bucket.

I hoped you enjoyed this gist. Feel free to fork it and to spread a word about it. Thanks.
