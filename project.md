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

Add permissions, including ‘amazonssmmanagedinstancecore’ and a custom trust policy.

Create a name for the role, then click create role. Creating this role allow our EC2 the permissions to interact with certain elements of system manager.

Create another role. Click create role. Choose custom trust policy then enter the following policy:

“Click Next, and add permissions to the Flow Log service, allowing it to interact with CloudWatch. Type ‘CLOUDWATCH LOGS’ and choose ‘CloudWatch Logs Full Access.’”

“Name your role and click ‘Create Role.’”

2. Launch EC2 Instances:
In EC2 UI, click “Create Instance.”
Name the instance, select “Amazon Linux 2” as AMI.

Continue without a key pair, create a security group, and uncheck “Allow SSH from”.

Scroll to advanced details. Configure IAM instance profile. Launch two instances.

3. Test Instance Connectivity:

SSH into instances using Session Manager.

Bonus! Within the instance’s Session Manager use command “ip a” to obtain your instances’ IP Address. This will work for both instances.

Attempt to ping between instances, confirming initial lack of connectivity.

The instances lack connectivity.
4. Set Up CloudWatch Logs:
In CloudWatch UI, create a log group for better visibility.

Go to CloudWatch Logs, create a log group, set retention, and create.

5. Create VPC Flow Logs:
In VPC UI, select the target VPC.
Click “Actions,” then “Create Flow Log.”

Specify log details, destination log group, and attach the IAM role.
Create flow logs.

6. Analyze Flow Logs:
Return to instances, ping again, and ensure logs are generated.

Check CloudWatch Logs UI for the log stream related to the ENI (network interface) of the instances.


Return to the Instance’s Session Manager. Ping the IP address 8.8.8.8.

Return to your Flow Log group in the CloudWatch Logs UI. Ensure that the connection you just made is logged in the log group.

Type in the IP address of the instance you pinged earlier in the search bar (e.g., 172.31.20.239). Confirm that the source/requestor can successfully send traffic to the destination. This confirms that the source of the traffic is not the problem.

Check the Flow Logs for the IP address (e.g., 172.31.22.218) you’ve been pinging. Confirm that the destination is not responding to our source pings.

7. Adjust Security Group Rules:
Modify security group rules to allow ICMP traffic to connect the two demo instances.

Ping again, confirming successful connectivity.

Success! You have connected your EC2 Instances using security groups.
8. NACL Setup:
Navigate to the NACL section in the source instance’s network settings.
Proceed to “Network ACL” and click “Edit Network ACL Association.”

Change the rule to ensure it comes first. Set the rule number to 1. Modify the type to “All ICMP-IPv4” and explicitly deny outbound traffic.

Confirm connectivity within the subnet but deny outside traffic. We will ping our destination instance and 172.31.22.218. The destination instance should work because it is within the same subnet as the source instance, however the 8.8.8.8 IP should not work because our NACL is to allowing any outside traffic.

9. Check Flow Logs for NACL:
Return to the log group UI, view the relevant ENI, and check logs for specific events. Type 8.8.8.8 to check the exact logged event. Confirm that the NACL has affected the flow of traffic. As you can see, the NACL(firewall) has changed our flow of traffic.

10. Cleanup:
Delete all created resources to conclude the exercise.
Conclusion:
By following this tutorial, you’ve gained practical insights into working with VPC Flow Logs. The step-by-step approach covered IAM roles, EC2 instances, security groups, and NACL configurations. Understanding these concepts is crucial for effectively managing and troubleshooting network connectivity within AWS.
