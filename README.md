# Auto Scaling with Load Balancer

This project demonstrates the setup of an Application Load Balancer (ALB) integrated with an Auto Scaling Group (ASG) in AWS to create a highly available, fault-tolerant, and scalable architecture. Follow the steps below to deploy this setup in your AWS environment.

## Prerequisites

1. AWS Account: Ensure you have an active AWS account.

2. IAM Permissions: Sufficient permissions to create EC2 instances, Load Balancers, Auto Scaling Groups, Target Groups, and Security Groups.

3. AWS CLI or Management Console: Use either AWS Management Console or AWS CLI for setup.

## Architecture Overview

- Application Load Balancer (ALB): Distributes incoming traffic across multiple EC2 instances.

- Target Group: Contains the instances that ALB routes traffic to.

- Auto Scaling Group (ASG): Dynamically adjusts the number of EC2 instances based on defined policies.

- Security Groups: Manage inbound and outbound traffic for instances and ALB.

## Step-by-Step Instructions

# 1. Create a Target Group

    1.Log in to the AWS Management Console.

    2.Navigate to the EC2 Dashboard.

    3.Go to Target Groups under the Load Balancing section.

    4.Click Create Target Group.

     - Target type: Instances.

     - Protocol: HTTP.

     - Port: 80.

     - Health check path: /health (or the endpoint your application exposes).
    
    5.Click Next, review settings, and create the target group.

# 2. Create an Application Load Balancer

    1.In the EC2 Dashboard, navigate to Load Balancers.

    2.Click Create Load Balancer and select Application Load Balancer.

    3.Configure the following:

      - Name: Provide a meaningful name.

      - Scheme: Internet-facing (or Internal, based on your needs).

      - IP address type: IPv4.

      - Listeners: Add HTTP on port 80.

      - Availability Zones: Select the VPC and subnets to deploy ALB.

    4.Assign a security group allowing inbound HTTP traffic on port 80.

    5.Register the previously created target group to the listener.

    6.Review and create the ALB.

# 3. Launch a Template or Configuration for EC2 Instances

     1.Navigate to the Auto Scaling section in the EC2 Dashboard.

     2.Create a Launch Template:

      -  Provide a name for the template.

      - Choose an AMI (Amazon Machine Image).

      - Select an instance type (e.g., t2.micro).

      - Configure security groups to allow traffic from the ALB.

      - Add user data (optional): Include startup scripts to configure instances after launch.

# 4. Create an Auto Scaling Group

     1.From the Auto Scaling Groups section, click Create Auto Scaling Group.

     2.Provide the following:

         Name: A meaningful name for the ASG.

         Launch template: Use the template created earlier.

         VPC and subnets: Select the same as those used for ALB.

     3.Attach the previously created target group to the ASG.

     4.Define scaling policies:

        - Enable dynamic scaling policies to scale out/in based on metrics like CPU utilization.

        - Set the desired capacity, minimum, and maximum instance count.

     5.Enable ELB health checks to ensure only healthy instances are in the group.

     6.Review and create the ASG.

# 5. Test the Setup

     1.Access the ALB's DNS name in a browser to verify traffic is routed to the instances.

     2.Simulate load to test scaling policies (e.g., use a load testing tool).

     3.Verify that instances scale up and down based on the defined policies.

## Monitoring and Logging

     - Enable CloudWatch Metrics to monitor the ALB and ASG performance.

     - Enable Access Logs for the ALB to capture request details.

     - Use CloudTrail for auditing API activity.

## Cleanup

  To avoid unnecessary charges, ensure you delete all created resources when no longer needed:

     - Delete the Auto Scaling Group.

     - Terminate EC2 instances.

     - Delete the Load Balancer.

     - Delete the Target Group.

     - Remove any associated Security Groups.

## References

- [AWS Application Load Balancer Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) 

- [AWS Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
