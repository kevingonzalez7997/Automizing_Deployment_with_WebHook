<!DOCTYPE html>
<html>
<head>
    <h1>ReadMe</h1>
</head>
<body>

<h2>Purpose</h2>
<p>
    The purpose of this project is to automate the deployment of our application using Elastic Beanstalk. Our code is sourced directly from GitHub, and the deployment process is tested and streamlined using Jenkins, which runs on an EC2 instance. An essential component of this project is the webhook, which further automates the process. Any changes made in GitHub and committed will trigger automatic deployments.
</p>

<h2>Prerequisites</h2>
<p>
    Before you begin, ensure that you meet the following prerequisites to minimize the chances of encountering issues, especially during Jenkins testing:
</p>
<ul>
    <li>Update your system by running the following commands to ensure everything is up to date:</li>
</ul>
<pre>
sudo apt update
sudo apt upgrade
</pre>
<ul>
    <li>Launch an EC2 instance on AWS</li>
    <li>Update and upgrade packages on the EC2 instance</li>
    <li>Install "python3.10-venv"</li>
    <li>Install "python-pip" (this is essential to install before proceeding)</li>
    <li>Install "unzip"</li>
    <li>Install "python3-pip"</li>
</ul>

<h3>Jenkins</h3>
<p>
    In Jenkins, start by creating a new item. For this project, the multibranch pipeline is used to select the desired branch for the pipeline.
</p>

<p>
    Configure security and credentials to enable Jenkins to extract the code directly from GitHub.
</p>

<p>
    Once credentials have been added, as well as the repository link, Jenkins can pull the code, build, and test it.
</p>
<p>Run and test</p>

<h3>AWS Credentials</h3>
<p>
   In AWS, access IAM roles, you can search for it. Find the current user, go to Security Credentials, and make and download your keys.
</p>
<p>
    Once you have the credentials, you can configure AWS.
</p>

<h3>AWS EB CLI install on Jenkins</h3>
<p>
    Create a login for Jenkins as EB CLI will be installed on Jenkins.
</p>
<p><code>sudo su - jenkins -s /bin/bash</code> to log in to Jenkins via the terminal. This is why we created the login earlier.</p>
<p>Install EB CLI in Jenkins using <code>pip install awsebcli --upgrade</code>, and then link it to the bin directory. You can test it by running <code>eb --version</code>.</p>
<p>Ensure that you have the IAM role for Elastic Beanstalk.</p>
<p>After configuring AWS and logging in with private and public keys, you should be in Jenkins. CD into the workspace and then the main branch of the repository.</p>
<p>In the terminal, run <code>eb init</code> to initialize Elastic Beanstalk.</p>
<p>Finally, run <code>eb create</code>. Congratulations, your Elastic Beanstalk environment should now be set. Once it finishes, you can see the application URL on the third-to-last line.</p>

<h3>Jenkins with Git</h3>
<p>In GitHub, navigate to the repository and access the Jenkinsfile.</p>
<p>Add the following stage in Jenkins:</p>
<pre>
<code>
stage ('Deploy') { 
    steps { 
        sh '/var/lib/jenkins/.local/bin/eb deploy' 
    } 
}
</code>
</pre>
<p>This command is used in Jenkins to deploy the Elastic Beanstalk application.</p>
<p>You can redeploy the app to ensure no syntax errors were made while adding the new code.</p>

<h3>Webhook</h3>

<p>In GitHub, go to the repository page and navigate to settings.</p>
<p>From here, select "Webhook" and add the webhook URL with the application's public IP.</p>
<p>To verify, click on "Recent Deliveries." You should see a ping call and a return error code 200, indicating success.</p>
<p>To fully test it, make changes in GitHub while Jenkins is open and observe what happens.</p>
<p>As soon as you commit in GitHub, Jenkins is automatically triggered and deploys the changes.</p>
<p>This is now a working pipeline that deploys automatically with the help of Jenkins and webhooks!</p>

<h3>IAM</h3>

<p>Two roles are needed to run: one for Elastic Beanstalk and one for the EC2 instance.</p>
<p>The necessary permissions are AWSElasticBeanstalkWebTier, WorkerTier, and MulticontainerDocker.</p>



![Screenshot 2023-09-16 090536](https://github.com/kevingonzalez7997/Deployment3/assets/59447523/fb792d17-eedf-495d-b8bf-5c0faf4f0c9e)
<p>This is our console after we commit a change on our github account.</p>
<p>If everything is running correctly you should be able to see it uploading the new code</p>
<p>Then it deploys it and gives confirmation</p>
</body>
</html>

