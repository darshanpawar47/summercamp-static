# summercamp-static

Let’s walk through the step-by-step implementation to set up a GitHub Actions CI/CD pipeline that:

Builds and tests your app

Creates a Docker image

Pushes it to Amazon ECR

Deploys it to an EC2 instance using SSH and Docker

 STEP 1: Set Up AWS Services
🔹 1.1 Create an ECR Repository
Go to Amazon ECR Console:

Click "Create Repository"

Name it, e.g., my-app

Note the full ECR URI: aws_account_id.dkr.ecr.region.amazonaws.com/my-app

🔹 1.2 Create an IAM User
Go to IAM > Users → Create a user (e.g., github-actions-user)

Attach policy: AmazonEC2FullAccess, AmazonEC2ContainerRegistryFullAccess

Save the Access Key ID and Secret Access Key

✅ STEP 2: Configure Your EC2 Instance
🔹 2.1 Launch an EC2 Instance
Choose Amazon Linux 2 or Ubuntu

Enable Auto-assign public IP

Create/download a new .pem key (SSH key)

🔹 2.2 Install Docker on EC2
SSH into EC2 and run:

For Amazon Linux 2:

sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
For Ubuntu:

sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker $USER
Then:
sudo reboot

✅ STEP 3: Add Secrets to GitHub
In your repo → Settings > Secrets and variables > Actions > New repository secret, add:

Secret Name	Value
AWS_ACCESS_KEY_ID	From IAM user
AWS_SECRET_ACCESS_KEY	From IAM user
AWS_REGION	e.g. us-east-1
ECR_REPOSITORY	e.g. my-app
EC2_HOST	e.g. ec2-user@your-ec2-public-ip
EC2_SSH_KEY	Paste the private SSH key content or base64-encoded key
DOCKER_IMAGE_NAME	Optional (e.g. my-app)

✅ STEP 4: Add GitHub Actions Workflow
Inside your repo, create:

mkdir -p .github/workflows
touch .github/workflows/deploy.yml
Paste the full pipeline code provided in the previous message.

Make sure your Dockerfile is in the project root and ready to build the app.

✅ STEP 5: Push Code to Trigger the Pipeline
Push your code to the main branch:

git add .
git commit -m "Add CI/CD pipeline"
git push origin main
