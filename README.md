# WebHook Deployment 3
September 17. 2023

Kevin Gonzalez

## Purpose:

To automate the deployment process when changes are committed to GitHub.

In our previous setup, Jenkins was used in automating the deployment process for our URL shortener, offering a significant improvement over manual deployment. However, it lacked the ability to handle updates automatically. In this new version, the update process is automated and triggered by GitHub through the addition of WebHook. Elastic Beanstalk is also installed for scalability management, allowing our application to adapt dynamically to changing workloads and resource requirements.

## Prerequisites:
- Before you begin, ensure that you meet the following prerequisites to minimize the chances of encountering issues, especially during Jenkins testing:
- Update your system by running the following commands to ensure everything is up to date
    - 'sudo apt update'
    - 'sudo apt upgrade'
- Install "python3.10-venv"
- Install "python-pip"
- Install "unzip"
- Install "python3-pip"

## Steps:

### 1. Jenkins running on an EC2 instance

- Jenkins is used to pull the code directly from the GitHub repository. Then it will build, test, and deploy the application
- Configure security and credentials to enable Jenkins to extract the code directly from GitHub(login). Also, ensure to use the correct repo link.

### 2. AWS Credentials

- In order to access AWS and make changes the correct permissions need to be set up. This will ensure the security of the deployment
- In AWS, access IAM roles, you can search for it. Find the current user, go to Security Credentials, and make and download your keys.
- AWS_ACCESS_KEY and AWS_SECRET_KEY should be saved as they will be required.

### 3. AWS EB CLI install on Jenkins

- EB CLI is used for resource management and code deployment. It will be installed on the instance, make sure to have credentials ready.
- The following will download the install package unzip it and install it. Lastly, we'll use the keys generated previously to configure AWS
    - `curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"'
    - `unzip awscliv2.zip'
    - `sudo ./aws/install'
    - `aws configure'

- Ensure that you have the IAM role for Elastic Beanstalk
- After configuring AWS and logging in with private and public keys, you should be in Jenkins. CD into the workspace and then the main branch of the repository
- Run
    - 'eb init'
    - 'eb create'

### 4. IAM Roles

- Two roles are needed to run: one for Elastic Beanstalk and one for the EC2 instance
- The necessary permissions are AWSElasticBeanstalkWebTier, WorkerTier, and MulticontainerDocker

### 5. Jenkins with Git

- In GitHub, navigate to the repository and access the Jenkinsfile.</p>
- Add the following stage in Jenkins:
<pre>
<code>
stage ('Deploy') { 
    steps { 
        sh '/var/lib/jenkins/.local/bin/eb deploy' 
    } 
}
</code>
</pre>
- This command is used in Jenkins to deploy the Elastic Beanstalk application
- You can redeploy the app to ensure no syntax errors were made while adding the new code

### 6. Webhook

- In GitHub, go to the repository page and navigate to settings
- Select "Webhook" and add the webhook URL with the application's public IP
- To verify, click on "Recent Deliveries." You should see a ping call and a return error code 200, indicating success
- To thoroughly test it, make changes in GitHub while Jenkins is open and observe what happens
- As soon as you commit in GitHub, Jenkins is automatically triggered and deploys the changes
- This is now a working pipeline that deploys automatically with the help of Jenkins and webhooks!

## TroubleTroubleshooting

- Some of the issues that are likely to happen are requirements errors
- Before trying to use a tool, it must be installed correctly beforehand
- Ensure that EB CLI and Python packages are both installed
- IAM roles need to be set up correctly in order to gain access when resources are needed
  
![Screenshot 2023-09-16 090536](https://github.com/kevingonzalez7997/Deployment3/assets/59447523/fb792d17-eedf-495d-b8bf-5c0faf4f0c9e)

## Conclusion
The main objective was to enhance deployment efficiency while ensuring scalability and security. By installing WebHooks with Jenkins, we were able to automate deployment triggered by GitHub commits. AWS Elastic Beanstalk provides scalability management. Security is handled with AWS IAM roles. The Jenkins pipeline, configured through the GitHub repository's Jenkinsfile, deploys our application completely automatically when changes are committed. 
