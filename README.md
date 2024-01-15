### SNS Notifications

Getting notified of failed builds in Code Pipeline

1. Create and subscribe to an SNS Topic
   - Create an SNS Topic on the AWS console
   - Create a subscription and confirm the email
2. Create the CloudWatch Rule
   - Rule type - Rule with an event pattern
   - Event source - AWS events or ...
   - Creation method - Use pattern form
   - Event pattern: AWS services - CodePipeline - CodePipeline Pipeline Execution State Change - Specific tasks - FAILED
   - After the above step, the JSON must have been populated, click on edit pattern and add "pipeline": ["pipeline1","pipeline2", ...] key-value pair
   ```json
   {
     "source": ["aws.codepipeline"],
     "detail-type": ["CodePipeline Pipeline Execution State Change"],
     "detail": {
       "pipeline": ["Pipeline-name-1", "pipeline-name-2"],
       "state": ["FAILED"]
     }
   }
   ```
   - Target will be the SNS topic you created earlier
   - Additional settings might include what you want to see in the email eg. providing a constant JSON text
   - Then review and create

### Jenkins Slave server migration

1. Create your required EC2 instances
   **Note**
   - The Instance type, plugins like JavaMelody can be used to check memory, cpu and lots more of jenkins nodes - I am familiar on how it works but we gonna try it
   - The master server should be accessible. Configure the security group appropriately e.g., port 22 for SSH
   - After launching the instances, install Java and any other dependency - Version of java should be same with that of the Master node and making sure all Java config are made
2. Copying one server to the other
   - Copy the jenkins folder and other files from the prem server to the new EC2 instance - cp source dest eg cp jenkins_folder root@ip/home/. Make sure the paths are similar
   - On the Jenkins master, create new nodes and provide the hostname which is the private IP address of an instance, other configuration like ssh access to the new EC2 instance, launch type ....

- Other Steps

  - There might be a possibility of updating the existing node and provide them with the new instance connection details instead of creating a new node where we can edit the configuration of the existing on-premises node. The launch method can be updated to use "Launch slave agents via SSH." and enter the EC2 instance details (hostname, credentials, etc.).

- This is a general overview of how it might work, it won't be this straight forward of course!
