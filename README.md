CI/CD Pipeline using ECR, GitHub, ECS, CodePipeline, and Load Balancer

# Step 1: Create Required Repositories in ECR

Install AWS CLI: Download and install the AWS CLI from <https://aws.amazon.com/cli/>.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image001.png?raw=true)

Configure AWS CLI: Create an IAM user or get the access key and secret key for the root user. Run the following command and enter your AWS credentials:  
aws configure

Create ECR Repository: Navigate to the AWS Management Console. Open the Amazon ECR console. Create a new repository to store your Docker images.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image002.png?raw=true)

Authenticate Docker to ECR: Get the login command from the ECR console and run it in your terminal:

aws configure

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image005.png?raw=true)

aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image007.png?raw=true)

# Step 2: Build and Push Docker Image

Build Docker Image: Build your Docker image using the Dockerfile.  
docker build -t your-image-name .

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image008.png?raw=true)

Tag Docker Image: Tag the Docker image with your ECR repository URI.  
docker tag your-image-name:latest aws_account_id.dkr.ecr.region.amazonaws.com/your-repo-name:latest

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image011.png?raw=true)

Push Docker Image to ECR: Push the tagged image to the ECR repository.  
docker push aws_account_id.dkr.ecr.region.amazonaws.com/your-repo-name:latest

# Step 3: Set Up GitHub Repository

Initialize GitHub Repository: Initialize a Git repository and add your remote GitHub repository.  
git init  
git remote add origin <https://github.com/your-username/your-repo-name.git>

Create Branches: Create the following branches:  
git checkout -b develop  
git checkout -b pre-develop  
git checkout -b release

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image012.png?raw=true)

Enable Branch Protection Rules: Go to your GitHub repository settings and set up branch protection rules.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image013.png?raw=true)
# Step 4: Configure AWS CodePipeline

Create a Pipeline: Open the AWS CodePipeline console and create a new pipeline. Provide a pipeline name and select the service role.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image014.png?raw=true)


Add Source Stage: Select GitHub (Version 2) as the source provider and connect your GitHub repository. Select the repository and branch (e.g., release).

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image015.png?raw=true)

Click connect to GitHub(Version 2) if you don’t have a project click Connect to GitHub

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image016.png?raw=true)

Provide a connection name and click on Connect to GitHub.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image017.png?raw=true)

Provide the required Authorization

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image019.png?raw=true)

Select the required repositories and click install

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image020.png?raw=true)

Approve it your GitHub ID will be displayed click connect

When you complete the above task your github id will be displayed in the pipeline.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image022.png?raw=true)

Select the Repository and Branch

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image024.png?raw=true)

Update the trigger

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image025.png?raw=true)

Output artifact format

Select code pipeline default

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image026.png?raw=true)

Add Build Stage: Select AWS CodeBuild as the build provider. Create a new build project and configure it to use the buildspec.yaml file from your repository.

select code build.

Create a new project by clicking the create project button.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image027.png?raw=true)

Provide a project name

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image028.png?raw=true)

Provide a role name

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image029.png?raw=true)

Select buildspec file and click continue to code pipeline

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image030.png?raw=true)

Select the project and click next

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image031.png?raw=true)

Till we create ECS skip the deployment stage.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image032.png?raw=true)

Click on Skip

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image033.png?raw=true)

The build will fail due to newly created IAM user doesn’t have the required permissions.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image034.png?raw=true)

Click on view details to view.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image035.png?raw=true)

To find the IAM role go to Build details and click on Service role

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image037.png?raw=true)

Click on add permissions

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image038.png?raw=true)

Add the EC2InstanceProfileForImageBuilderECRContainerBuilds. Add the required permission to the user

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image039.png?raw=true)

Go back to the release pipeline and click release change

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image040.png?raw=true)

Click on Release

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image041.png?raw=true)

The build will succeed.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image042.png?raw=true)

# Step 5: Set Up CodeBuild

Create Build Project: Provide a project name and select the build environment. Specify the buildspec file:  
\`\`\`yaml  
version: 0.2  
phases:  
install:  
runtime-versions:  
docker: 18  
pre_build:  
commands:  
\- $(aws ecr get-login --no-include-email --region $AWS_DEFAULT_REGION)  
build:  
commands:  
\- docker build -t your-image-name .  
\- docker tag your-image-name:latest aws_account_id.dkr.ecr.region.amazonaws.com/your-repo-name:latest  
post_build:  
commands:  
\- docker push aws_account_id.dkr.ecr.region.amazonaws.com/your-repo-name:latest

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image046.png?raw=true)

# Step 6: Create ECS Cluster and Task Definition

Create ECS Cluster: Open the ECS console and create a new cluster with Fargate (serverless) launch type.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image047.png?raw=true)

Select Fargate (serverless) application.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image048.png?raw=true)

Click Create button

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image049.png?raw=true)

Create Task Definition: Define the task definition with container details, environment variables, and resource requirements.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image050.png?raw=true)

Container details

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image051.png?raw=true)

Add environment file and variables according to the requirement

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image052.png?raw=true)

Create Service

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image053.png?raw=true)

Fill the family and Task Definition revision version

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image054.png?raw=true)

Update the networking requirements.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image055.png?raw=true)

# Step 7: Configure Load Balancer

Create Load Balancer: Open the EC2 console and create a new load balancer. Configure the load balancer to direct traffic to the ECS service.

Select the correct VPC and subnets and Security groups.

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image056.png?raw=true)

Click on create button

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image057.png?raw=true)

Go to Configuration and networking

Get the DNS from the network configuration

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image058.png?raw=true)

# Step 8: Deploy to ECS

Create ECS Service: Create a new service in the ECS console using the task definition. Configure the service to use the load balancer.

Update CodePipeline: Add a deploy stage to your pipeline, specifying ECS as the deployment provider. Select the ECS cluster and service created earlier.

For the deployment go back to pipelines and update the pipeline.

Image definitions file should be taken from the buildspec.yaml from the codebase

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image059.png?raw=true)

Click next

![alt text](https://github.com/malindrarodrigo/IJSE-ai/blob/main/CI_CD_Pipeline_Tutorial_files/image060.png?raw=true)

# Advantages of CI/CD Pipeline

- Automation: Automates the entire build, test, and deployment process, reducing manual intervention and errors.
- Consistency: Ensures consistent deployment processes across environments.
- Speed: Accelerates the release cycle, enabling quicker updates and faster time-to-market.
- Scalability: Easily scales to handle increased load and larger codebases.

# Further Improvements

- Testing: Integrate automated testing (unit, integration, and end-to-end tests) into the pipeline.
- Monitoring: Add monitoring and logging to track the performance and health of applications.
- Security: Implement security checks and vulnerability scanning in the pipeline.





