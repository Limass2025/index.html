This is a GITFILE SUBMISSION outlining the process of configuring Auto Scaling with an Application Load Balancer (ALB) using a Launch Template in AWS. This setup ensures that your application can automatically scale to handle varying workloads efficiently.

***

### Part 1: Create a Launch Template

A **Launch Template** is a saved configuration for launching EC2 instances. It acts as a blueprint for your instances, ensuring consistency.

1.  **Navigate to the EC2 Dashboard:**
    * Log in to the AWS Management Console and go to the **EC2 service**.
    * In the left navigation pane, click on **Launch Templates**.

    ![alt text](<Screenshot 2025-08-07 161230.png>)

2.  **Create the Launch Template:**
    * Click the "**Create launch template**" button.
    * Provide a **name** and description for your template.
    * Under **Application and OS Images (Amazon Machine Image)**, select an AMI for your instances (e.g., Amazon Linux 2 AMI).
    * Choose an **Instance type** (e.g., `t2.micro`).
    * Under **Key pair (login)**, select an existing key pair for SSH access.
    * Under **Network settings**, select a security group that allows inbound traffic on the necessary ports (e.g., HTTP on port 80).
    * In the **Advanced details** section, you can add a **User data** script to automatically install and configure a web server on the instances as they launch.

   ![alt text](<Screenshot 2025-08-07 161632.png>)
3.  **Finalize Creation:**
    * Review your settings and click "**Create launch template**" at the bottom of the page.

   ![alt text](<Screenshot 2025-08-07 172754.png>)

***

### Part 2: Set Up an Auto Scaling Group

An **Auto Scaling Group** manages a collection of identical instances, launching or terminating them to meet a desired capacity.

1.  **Navigate to Auto Scaling Groups:**
    * From the EC2 dashboard, click on **Auto Scaling Groups** in the left navigation pane.
    * Click the "**Create Auto Scaling group**" button.

 ![alt text](<Screenshot 2025-08-07 161731.png>)

2.  **Configure the Auto Scaling Group:**
    * Provide a **name** for your Auto Scaling group.
    * On the first step, "**Choose launch template or configuration**," select "**Launch template**" and choose the template you just created from the dropdown menu. Click **Next**.

   ![alt text](<Screenshot 2025-08-07 171137.png>)
3.  **Choose Network and Subnets:**
    * On the "**Choose instance launch options**" page, select your **VPC**.
    * Choose the **subnets** where you want your instances to launch. Use subnets in multiple Availability Zones for high availability. Click **Next**.

  
***

### Part 3: Attach an ALB to the Auto Scaling Group

An **ALB** distributes incoming application traffic across the instances in your Auto Scaling Group, ensuring no single instance is overwhelmed.

1.  **Configure Load Balancing:**
    * On the "**Configure advanced options**" page, select **Attach to an existing load balancer**.
    * Choose the **Target group** associated with your ALB. The Auto Scaling group will register its instances with this target group.

    
![alt text](<Screenshot 2025-08-07 171502-1.png>)
2.  **Health Checks:**
    * Under **Health checks**, ensure that **ELB** is selected as the health check type. This will use the load balancer's health checks to determine the state of the instances.

 

3.  **Set Group Size and Scaling Policies:**
    * On the "**Configure group size and scaling policies**" page, define the following:
        * **Desired capacity:** The number of instances to have running immediately.
        * **Minimum capacity:** The smallest number of instances the group can scale down to.
        * **Maximum capacity:** The largest number of instances the group can scale up to.
    * Under **Scaling policies**, choose "**Target tracking scaling policy**".
    * Configure a policy based on a metric like **Average CPU utilization**. Set a target value (e.g., 60%). This policy will automatically adjust the group's size to keep the average CPU utilization around that target.
    * Click **Next**.
![alt text](<Screenshot 2025-08-07 171137-1.png>)
  ![alt text](<Screenshot 2025-08-07 171147.png>)
  
4.  **Review and Create:**
    * Continue through the remaining steps, reviewing the configuration.
    * On the final page, click "**Create Auto Scaling group**."

  ![alt text](<Screenshot 2025-08-07 171529.png>)

***

### Part 4: Test Auto Scaling

1.  **Generate Traffic:**
    * SSH into one of the instances launched by the Auto Scaling group.
    * Install the `stress` tool to generate load on the instance's CPU:
        * `sudo amazon-linux-extras install epel -y`
        * `sudo yum install stress -y`
    * Run the stress command to simulate high CPU usage. The number after `-c` indicates the number of workers. For example, to stress 4 CPU cores: `stress -c 4`
![alt text](<Screenshot 2025-08-07 172025.png>)

2.  **Monitor Auto Scaling:**
    * Go back to the **Auto Scaling Groups** dashboard in the AWS console.
    * Monitor the **Activity history** and **Monitoring** tabs. You should see a new instance being launched after the CPU utilization of the existing instance(s) crosses the threshold defined in your scaling policy.
    * After terminating the stress test (`Ctrl+C`), the Auto Scaling group will eventually scale in, terminating the extra instances.
![alt text](<Screenshot 2025-08-07 172433.png>)
   