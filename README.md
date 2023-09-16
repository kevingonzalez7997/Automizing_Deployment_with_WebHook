<!DOCTYPE html>
<html>
<head>
  <H1>ReadMe</H1>
</head>
<body>

<h2>Purpose</h2>

<p>In this project, the end goal is to deploy our application automatically using Elastic Beanstalk. The code is sourced from github directly. Additionally, the program is tested through Jenkins, which runs on an EC2 instance to ensure the code is functioning correctly.</p>

<h2>Issues</h2>

<p>Before you start, there are a set of prerequisites to minimize the chances of encountering bugs, especially during testing in Jenkins:</p>

<ul>
  <li>Always run the following commands to ensure everything is updated before installation:</li>
</ul>

<pre>
sudo apt update
sudo apt upgrade
</pre>





<ul>
  <li>Launch an EC2 instance on AWS</li>
  <li>Update and/or upgrade packages</li>
  <li>Install "python3.10-venv""</li>
  <li>Install "python-pip" (this is important to install before proceeding)</li>
  <li>install "unzip"</li>
  <li>install "python3-pip</li>
</ul>

<h3>AWS EB CLI</h3>
<p>AWS EB CLI should be installed before running the multibranch pipeline. Create a login for Jenkins as we will be installing it on jenkinks</p>


<h3>Jenkins</h3>
<p>In Jenkins start by creating a new item. For this project, the multibranch pipeline is going to be used. This allows the user to pick which branch they would like to apply the pipeline to</p>

<p>From there the security and credentials must be added as we will extract the code directly from github.</p>

<p>Once the credentials have been added the code can be built and tested</p>

<p> Do a pip install for awsebcli and lastly export. you can test by  running 'eb --version</p>
<p>make sure you have the IAM role for EB</p>
<p> once you have configured the credentials we got earlier you can now access jenkins and change into the pipeline directory</p>
<p> eb create and in the 3 to last line you can see the application URL </p>






<h3>IAM</h3>

<p>Two roles are needed in order to run: one for Beanstalk and one for the EC2 instance.</p>

<p>The necessary permissions are AWSElasticBeanstalkWebTier, workerTier, and MulticontainerDocker.</p>

<h3>Setting up ElasticBeanstalk</h3>

<p>Now let's set up ElasticBeanstalk to manage traffic automatically:</p>

<ul>
  <li>Create an application:</li>
  <ul>
    <li>Give it a name</li>
    <li>Pick the platform (Python 3.9)</li>
    <li>Upload the zipped file that was downloaded from Jenkins (upload local file)</li>
  </ul>
  <li>Select the ElasticEC2 profile created in the IAM section</li>
  <li>Pick the default VPC</li>
  <li>Choose the availability zone (us-east-1a)</li>
  <li>Pick storage type (general purpose SSD)</li>
  <li>Set minimum storage to 10GB</li>
  <li>Choose instance type: “T.2 MICRO”</li>
  <li>Submit and check health status</li>
</ul>

<p>If everything worked correctly, you should now have access to the domain.</p>

</body>
</html>
