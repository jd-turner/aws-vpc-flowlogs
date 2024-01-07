Summary: Gain hands-on experience working with VPC Flow Logs to understand and troubleshoot network traffic between EC2 instances. The tutorial covers creating IAM roles, launching EC2 instances, configuring security groups, and analyzing flow logs to diagnose and address connectivity issues.

AWS Resources Used:
EC2 Instances
IAM Roles
Security Groups
VPC Flow Logs
Step-by-Step Guide:
1. Create IAM Roles:
In IAM UI, click on “Create Role.”
Select “AWS service,” choose “EC2” in the dropdown, and proceed.

![1_ZvQ4f1VMDi5_d3wAjC5XcA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/a468463c-ced4-4cd1-9418-562e559d2450)

Add permissions, including ‘amazonssmmanagedinstancecore’ and a custom trust policy.

![1_5kfDD74jY47uYcJCPGGRHw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/ff831f62-59a3-4cee-a9d1-71cf1c89bf38)

Create a name for the role, then click create role. Creating this role allow our EC2 the permissions to interact with certain elements of system manager.

![1_V9NFZIaLx8rSrE2z_PZVow](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/56ac087d-fc32-48e7-9fa7-c12cdd4a3014)

Create another role. Click create role. Choose custom trust policy then enter the following policy:

![1_2T2-_q8ZAR-oEatLnRAHhQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/cb39db00-96df-456d-b1b9-f9768fc9c568)

“Click Next, and add permissions to the Flow Log service, allowing it to interact with CloudWatch. Type ‘CLOUDWATCH LOGS’ and choose ‘CloudWatch Logs Full Access.’”

![1_FnBSwAhpStML8TNX0SyiLA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/0e56325c-e516-458d-8f0a-598dc81ee303)

“Name your role and click ‘Create Role.’”

![1_FnBSwAhpStML8TNX0SyiLA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/0e56325c-e516-458d-8f0a-598dc81ee303)

2. Launch EC2 Instances:
In EC2 UI, click “Create Instance.”
Name the instance, select “Amazon Linux 2” as AMI.

![1_lEOCD3Tlhbp9tT6Q7NEZ1Q](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/8a60df4e-4bc5-4d45-8bfc-2a2d611fe0fd)

Continue without a key pair, create a security group, and uncheck “Allow SSH from”.

![1_V3E-Op-Iuj_nstLG1c0B-w](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/90b43bcd-0e3f-4fd0-bc48-3bcb51d97570)

Scroll to advanced details. Configure IAM instance profile. Launch two instances.

![1_7iBWm2DYutKxa-yO0MMcUg](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/7038d59b-f147-4022-acdc-ee79d4d65e9c)

3. Test Instance Connectivity:

![1_4hhb8--7-5Awi4FRxjvHCw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/6297bfde-f0b1-4f16-85e5-36aba28696d4)

SSH into instances using Session Manager.

![1_Nc9nk0DPXIpAjJ5oEGHJMg](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/a88c71db-2bd6-46e7-9b00-16b507e11f9e)

Bonus! Within the instance’s Session Manager use command “ip a” to obtain your instances’ IP Address. This will work for both instances.

![1_43PCy9pcTem-yPLVv20zEA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/3cc478c1-303d-461b-982c-3eef21a7c335)

Attempt to ping between instances, confirming initial lack of connectivity.

![1_SbThLziU8xmLEG3-buy7uA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/0b6250f4-1bc5-4bc2-98b0-ddc0a2f9ec42)

The instances lack connectivity.

4. Set Up CloudWatch Logs:
In CloudWatch UI, create a log group for better visibility.

![1_v9s7bJ9-RkzLinDn2Q7Uig](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/54a061ed-af5a-47b0-9750-e198f4605cd6)


Go to CloudWatch Logs, create a log group, set retention, and create.

![1_Vu5CHBqwh1Ev2pkmN10vUw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/044e06ee-33aa-4e88-a52b-3ac5b5416574)


5. Create VPC Flow Logs:
In VPC UI, select the target VPC.
Click “Actions,” then “Create Flow Log.”

![1_GtBmEbTt2o6R0ge_bw64nw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/d6e0f917-0565-4acf-bb4f-7be55e42481e)

Specify log details, destination log group, and attach the IAM role.
Create flow logs.

![1_0yykd5YULVi27YQiDKPvZg](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/1ed600b6-e79b-4e3c-b33f-2baf81ea8eed)


6. Analyze Flow Logs:
Return to instances, ping again, and ensure logs are generated.

![1_5yDpLuO9hPxR1Cmo0gJUTQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/2006537e-62e5-4c79-9833-d9c2b0f27fc8)


Check CloudWatch Logs UI for the log stream related to the ENI (network interface) of the instances.

![1_Y5QJhmNLEzU2eQihNfxSWQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/42797d8c-9941-46f3-b684-2f4d636860bb)
![1_97hqt2_lSDrNcJGlYhq2CQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/f83ad99a-bfa2-489b-8036-1531a8fd1b4e)

Return to the Instance’s Session Manager. Ping the IP address 8.8.8.8.

![1_IesC4OmXrVpicfzbWyXj3A](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/ea6a10e6-5b8c-4be3-9862-0529b83df19e)


Return to your Flow Log group in the CloudWatch Logs UI. Ensure that the connection you just made is logged in the log group.

![1_bkohRV2pHnWnve-Sdw9csA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/e4a1a035-488a-49ff-988e-a1f97436761d)

Type in the IP address of the instance you pinged earlier in the search bar (e.g., 172.31.20.239). Confirm that the source/requestor can successfully send traffic to the destination. This confirms that the source of the traffic is not the problem.

![1_z3fydo8KvXDF6pfBWAMvSA](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/1a636ad2-c930-48bd-9651-b5ef9f530d6f)

Check the Flow Logs for the IP address (e.g., 172.31.22.218) you’ve been pinging. Confirm that the destination is not responding to our source pings.

![1_ntZiajL6dSp3-lB1BvdzeQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/06d4e209-e59c-448f-8e5c-8cec5a9cf472)

7. Adjust Security Group Rules:
Modify security group rules to allow ICMP traffic to connect the two demo instances.

![1_aEKweF85skKrYCiW89WYwQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/2684283d-31b1-46b7-9a26-e735422b5555)

Ping again, confirming successful connectivity.

![1_FKKWHi6uj7Y0y2fYM2ewdQ](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/c2a09738-6973-4125-9ed4-3add57198a28)


Success! You have connected your EC2 Instances using security groups.
8. NACL Setup:
Navigate to the NACL section in the source instance’s network settings.
Proceed to “Network ACL” and click “Edit Network ACL Association.”

![1_tw4YzVwXyTMznZiIWv7mvw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/55861350-750a-4c13-9de7-6a1cd423bc94)


Change the rule to ensure it comes first. Set the rule number to 1. Modify the type to “All ICMP-IPv4” and explicitly deny outbound traffic.

![1_6LxWCus34K60qXVKJc0Qbw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/37049dc8-0db6-41fa-af87-c21d9f643a05)


Confirm connectivity within the subnet but deny outside traffic. We will ping our destination instance and 172.31.22.218. The destination instance should work because it is within the same subnet as the source instance, however the 8.8.8.8 IP should not work because our NACL is to allowing any outside traffic.

![1_z7SCCvx8GmFSQGdQ7oqN9g](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/03e2a800-43d8-4861-b4e1-951acb850e2b)

9. Check Flow Logs for NACL:
Return to the log group UI, view the relevant ENI, and check logs for specific events. Type 8.8.8.8 to check the exact logged event. Confirm that the NACL has affected the flow of traffic. As you can see, the NACL(firewall) has changed our flow of traffic.

![1_b9Y1x-uaNCSDf1SWYn2Tmw](https://github.com/jd-turner/aws-vpc-flowlogs/assets/129380235/7ba833af-6c6e-4b14-905f-1e56d142a7f3)

10. Cleanup:
Delete all created resources to conclude the exercise.

Conclusion:
By following this tutorial, you’ve gained practical insights into working with VPC Flow Logs. The step-by-step approach covered IAM roles, EC2 instances, security groups, and NACL configurations. Understanding these concepts is crucial for effectively managing and troubleshooting network connectivity within AWS.
