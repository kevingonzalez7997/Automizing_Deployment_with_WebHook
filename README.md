# WebHook Deployment (3)
September 17. 2023

Kevin Gonzalez

## Purpose:

To automate the deployment process when changes are committed to GitHub.

In our previous setup, Jenkins was used in automating the deployment process for our URL shortener, offering a significant improvement over manual deployment. However, it could not handle updates automatically. In this new version, the update process is automated and triggered by GitHub through the addition of WebHook. Elastic Beanstalk CLI is also installed to further automate the deployment process.

## Prerequisites:
- Have EC2 instances running with Ubuntu
- Best practice have the system up to date before installing anything
     - `sudo apt update`
     - `sudo apt updgrade`
- Install "python3.10-venv"
- Install "python-pip"
- Install "unzip"
- Install "python3-pip"

## Steps:

### 1. Installing Jenkins on an EC2 instance
Jenkins is a popular open-source continuous integration (CI) server. It is used to build, test, and deploy software projects.
- Install Jenkins Server using instructions [Here](https://pkg.jenkins.io/debian/)
- You will need to get a key to set up Jenkins for the first time, run:
     -  `sudo cat  /var/lib/jenkins/secrets/initialAdminPassword`
- After accessing Jenkins through port 8080, install the suggested plug-ins

### 2. Connecting GitHub to Jenkins 
GitHub, a prominent version control platform, is where Jenkins retrieves code through cloning. It offers essential tools for version tracking, collaborative coding, and efficient code repository management
- create a new item, and select  Multi-branch pipeline
  - Jenkins Credentials Provider:
- Copy and import the Repository URL where the application source code is located
- User Name will be Github user and the password is the generated key in GitHub
  
### 3. AWS Credentials

- In order to access AWS and make changes, the correct permissions must be set up. This will ensure the security of the application
- In AWS
    -  Access IAM roles
    -  Find the current user
    -  Navigate to Security Credentials
    -  Create access key
    -  Download your keys
- AWS_ACCESS_KEY and AWS_SECRET_KEY should be saved as they will be required.

### 4. AWS EB CLI install on Jenkins
- EB CLI is used to optimize the deployment process by introducing automation. It will be installed on the instance, make sure to have credentials ready.
- The following will download the install package
- Unzip and run the install package
- Use the key generated previously to configure AWS
    - `curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
    - `unzip awscliv2.zip`
    - `sudo ./aws/install`
    - `aws configure`

- Ensure that you have the IAM role for Elastic Beanstalk
- After configuring AWS and logging in with private and public keys, you should be in the Jenkins instance. - CD into the 'workspace' and then the main branch of the repository
- Run the following commands to initialize elastic beanstalk
    - `eb init`
    - `eb create`
  

### 5. IAM roles

- IAM roles in AWS serve a crucial purpose in managing and securing access to AWS resources
- Two roles are needed to run: one for Elastic Beanstalk and one for the EC2 instance
- Starting with Elastic BeanStalk
  - Select type Elastic
  - Select Elastic BeanStalk
- For the EC2 start by selecting EC2
     - The necessary policies are AWSElasticBeanstalkWebTier, AWSElasticBeanstalkWorkerTier, and AWSElasticBeanstalkMulticontainerDocker

### 6. Installing deployment to the GitHub repository 

- In GitHub, navigate to the repository and access the Jenkinsfile.
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

### 7. Webhook

- In GitHub, go to the repository page and navigate to settings
- Select "Webhook" and add the webhook URL with the application's public IP
- To verify, click on "Recent Deliveries." You should see a ping call and a return error code 200, indicating success
- To thoroughly test it, make changes in GitHub while Jenkins is open and observe what happens
- As soon as you commit in GitHub, Jenkins is automatically triggered and deploys the changes
- This is now a working pipeline that deploys automatically with the help of Jenkins and webhooks!

## Observations

- Jenkins:
     1. Checkout SCM: Jenkins connecting to GitHub and cloning the most recent repository, using the key created with GitHub
     2. Build: Jenkins downloaded all the required files to run the program from GitHub repo
     3. Test: Jenkins ran a test file "test_app.py::test_home_page PASSED " and returned passed
     4. Deploy: Jenkins logs deployment and then uploads and returns complete status and success status
 
- Elastic Beanstalk:
    1. Creates Autoscaling group
    2. Creates Cloudwatch
    3. creates a load balancer
    4. deployment
- These are some of the advantages of using AWS tools. Certain aspects of the pipeline can be handed by Amazon automatically. An example is the load balancer and autoscaling to ensure the application is resilient.

- The logs can be viewed [Here](https://github.com/kevingonzalez7997/Automizing_Deployment_with_WebHook/tree/main/Results)

       
## TroubleTroubleshooting

- Some of the issues that are likely to happen are requirements errors
- Before trying to use a tool, it must be installed correctly beforehand
- Ensure that EB CLI and Python packages are both installed
- IAM roles need to be set up correctly in order to gain access when resources are needed
  
## Optimization 
In this deployment, there were many optimizations that were incorporated to create the pipeline into a CICD pipeline. The two major components that allowed this were webhook and EB CLI. This was an improvement from having to manually deploy the application after every change. However, there is still room for improvement to ensure the resiliency of the application. A monitor system can be incorporated to minimize downtime. We could also launch different clones of our program in different AZ to further increase its resistance.

## Conclusion
The main objective was to enhance deployment efficiency while ensuring scalability and security. By installing WebHooks with Jenkins, we were able to automate deployment triggered by GitHub commits. AWS Elastic Beanstalk CLI further assists with deployment and scalability. Security is handled with AWS IAM roles and security groups. The CICD pipeline, configured through the GitHub repository's Jenkinsfile, automatically deploys our application when changes are committed thanks to EB CLI. 
