
### Installing and Configuring the AWS CLI

The AWS CLI is the tool that allows you to interact with AWS services directly from your terminal.
![alt text](<Screenshot 2025-08-07 230733.png>)
1.  **Download and Install AWS CLI:**

      * The installation process varies by operating system on Windows, macOS. Follow the official instructions to download and install the CLI version 2.
      * For **windows**, the commands are:
        ![alt text](<Screenshot 2025-08-07 224427.png>)

        ![alt text](<Screenshot 2025-08-07 224248.png>)

        
2.  **Verify Installation:**

      * Open your terminal or Command Prompt and run the following command to check if the installation was successful. You should see the version information displayed.
        `aws --version`

   ![alt text](<Screenshot 2025-08-07 224545.png>)

3.  **Configure the AWS CLI:**

      * Run the `aws configure` command in your terminal.
      * When prompted, enter the **Access key ID** and **Secret access key** for the `automation_user`.
      * Specify your **Default region name** (e.g., `us-east-1`).
      * Set the **Default output format** (e.g., `json`).

      ![alt text](<Screenshot 2025-08-07 225439.png>)

      ![alt text](<Screenshot 2025-08-07 225620.png>)

4.  **Test the Configuration:**

      * To confirm that the CLI is configured correctly and can communicate with AWS, run a simple command like `aws ec2 describe-regions`. This command queries the EC2 API and lists all available regions. The output confirms successful authentication.
        `aws ec2 describe-regions --output table`

       
  ![alt text](<Screenshot 2025-08-07 230711.png>)
-----

### Project Summary

This project has solidified my understanding of the crucial security and authentication steps required before automating tasks on AWS. I learned the importance of **least-privilege access** by creating a dedicated IAM user with a specific policy. I gained practical experience in generating and securely managing programmatic access credentials. The project also covered the installation and configuration of the **AWS CLI**, a fundamental tool for interacting with AWS services. Finally, I demonstrated how to use the `aws configure` command to link a user's credentials to the CLI and tested the setup by making a successful API call. This process is the secure foundation for any script that will create or manage AWS resources.