# Complete-CI-CD-for-Netflix
### Intriduction
Step into the world of DevOps excellence with my Netflix clone project. Through a meticulously crafted CI/CD pipeline orchestrated by Jenkins, I've elevated the deployment process to new heights using Docker containers. But the journey doesn't end there. Dive deeper and discover how I've integrated cutting-edge monitoring tools like Prometheus, Node Exporter, and Grafana to ensure peak performance.
Security and code quality are paramount in today's landscape, which is why I've implemented a suite of tools including SonarQube, Trivy, and OWASP Dependency Check. These safeguards guarantee that your application remains secure and your code maintains the highest standards.
Embark on this DevOps adventure with me and unlock the potential for seamless, secure, and efficient software deployment.
## Steps
Code URL:- https://github.com/Teddydesta/Netflic_cicd.git

1. Configure a server for Jenkins, Sonarqube and Docker
2. Install Docker, Jenkins and Create a SonarQube Container
3. Integrate Sonarqube, Docker, Nodejs, OWASP with Jenkins
4. Configure a server for Prometheus and Grafana
5. Configure Email In Jenkins
6. Install Trivvy
7. Create TMDB API key
8. Jenkins Job Pipeline
## Step:- 1. Configure a server for Jenkins, Sonarqube and Docker
![image](https://github.com/user-attachments/assets/407de58d-ed66-4513-839d-e20bd1f2f087)
![image](https://github.com/user-attachments/assets/3bd1e4a1-bea5-4043-94d1-ff69f5f67c51)
**Connect to Virtual Machine**
![image](https://github.com/user-attachments/assets/37d9a83c-15cc-44a3-9997-4252bbb70cc9)
## Step: 2 Install Docker, Jenkins and Create a SonarQube Container
### 2. 1 Install Jenkins on the server

```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
 https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins
```


**copy the Ip address of virtual machine and paste on your web browser address bar with the port number**

![image](https://github.com/user-attachments/assets/73a1d8ff-1eb4-4566-82c4-e57ebfa88b85)

`/var/lib/jenkins/secrets/initialAdminPassword
`

![image](https://github.com/user-attachments/assets/cc82d041-beba-4792-8e6c-7b0eec3d09d1)
![image](https://github.com/user-attachments/assets/94f3439c-3812-4d1f-bf10-58589b87d3e0)

Our Jenkins Installed Successfully

![image](https://github.com/user-attachments/assets/1c67f3ed-eed4-4f63-8eeb-55b904e436a1)

### 2.2 Install Docker on the same server
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
```

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running:

![image](https://github.com/user-attachments/assets/244fde8f-70d5-4dc7-b55f-168289b2d198)

Download the following docker plugins: - Docker commons, docker pipeline, docker API, docker, docker-build-step

![image](https://github.com/user-attachments/assets/b9b619a1-8183-4dc2-b09b-ef17268dfc3d)

### 2. 3 Integrate Docker with Jenkins
To enable Jenkins to construct Docker images housing for your application and execute these images as containers, Docker specifics have been integrated into Jenkins.

![image](https://github.com/user-attachments/assets/29fddfff-d527-4b3e-b6e6-d8a2cd58c769)

### 2. 4 Create a SonarQube Container
**SonarQube** is a Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of your project. … Sonarqube also ensures code reliability, Application security, and reduces technical debt by making your code base clean and maintainable
**Pull the latest docker image from docker hub using the following command:-**

`docker run -d --name sonarq -p 9000:9000 sonarqube:lts-community`

![image](https://github.com/user-attachments/assets/74257c59-ecc4-424d-8038-af750475a62a)

Our sonarqube is installed successfully, you should see the image is running on port 9000 using the followind command:-

`docker ps`

![image](https://github.com/user-attachments/assets/9813f0fe-6138-48fe-b37f-9a6e1cb37e8b)

To get a SonarQube page copy and paste the public address of your server and sonar port(in this case :9000) in your preferred web browser.

![image](https://github.com/user-attachments/assets/9892431e-6bb5-4b3b-8a8a-5db5a75ddaa4)

**Sonar created successfully, you should a page like below**

![image](https://github.com/user-attachments/assets/ea4e5a95-8bd6-48eb-a805-a9e28f82f573)

Here we need a way to integrate SonarQube with jenkins, so for this purpose we will create a token.

A "**SonarQube token**" is a unique identifier used to authenticate with the SonarQube server, typically for analysis purposes or when using web APIs. It's a safer and more convenient way to authenticate compared to using a username and password, as it can be easily revoked if compromised
You can generate new tokens at User > My Account > Security.

![image](https://github.com/user-attachments/assets/111a6f81-616c-41f5-9dfb-95554d7de3f5)

To enable automatic code analysis triggering and promptly receive feedback on code quality, we'll utilize Webhooks. To set up a webhook, follow this **steps**:- **Administration** > **Configuration** > **Webhooks**

![image](https://github.com/user-attachments/assets/22ffd6dc-8f0a-4130-b43b-7151db8c1de2)

To configure your webhook, specify its name and the URL of your Jenkins instance, followed by "/sonarqube-webhook/" to denote its purpose for handling SonarQube-related requests. Once done, click the "create" button to finalize the setup.

![image](https://github.com/user-attachments/assets/34ec54bf-877f-4e29-a136-d5b5d21b586c)

**Here is the created weebhooks**

![image](https://github.com/user-attachments/assets/7f93d764-1acb-461b-a8e2-b48122fb0f80)

## Steps: 3 Integrate Sonarqube, Nodejs, OWASP with Jenkins
### 3.1 Download plugins
**Download** plugins like Sonarqube Scanner, Nodejs, OWASP dependency check
![image](https://github.com/user-attachments/assets/c0c488b9-e289-4abb-8cf2-911acbddced5)

### 3. 2 Integrate SonarQube with Jenkins
To ensure smooth integration between Jenkins and SonarQube, we'll incorporate the SonarQube server details within the Jenkins tool page. **Follow these steps:**

1. Go to the Jenkins Dashboard and select Manage Jenkins > Configure System.
2. In the SonarQube Servers section, click on Add "SonarQube".

![image](https://github.com/user-attachments/assets/3f5eb71e-8a09-4060-bb35-97ec749514d3)

### 3. 3 Integrate JDK with Jenkins
To ensure compatibility and uniformity across Java-based projects and to enable Jenkins to execute compilation and execution tasks efficiently, we will Incorporate JDK (Java Development Kit) details into Jenkins.

![image](https://github.com/user-attachments/assets/e5aaee5c-4e74-429c-b2fe-075e86aa7c6d)

### 3. 4 Integrate NodeJs with Jenkins
To optimize Jenkins for building, testing, and deploying the Netflix application, we'll incorporate Node.js details into Jenkins. This step is crucial given that the application is a Node.js-based one.

![image](https://github.com/user-attachments/assets/5c006061-19f2-42d1-bfa9-b2d550f2b3e7)

### 3. 5 Integrate OWASP dependency check with Jenkins
In order to detect and tackle potential vulnerabilities at an early stage of development and thus reduce security risks, dependency check information has been integrated into Jenkins. This inclusion not only elevates the overall quality of code but also fosters a secure software development lifecycle.

![image](https://github.com/user-attachments/assets/a14e1af2-ad48-46e3-a794-e69cd9b7456f)

## Step: 4 Configure an ubuntu server for Prometheus and Grafana
![image](https://github.com/user-attachments/assets/f02dff49-57f2-4b44-b46c-55c4df0695d9)
![image](https://github.com/user-attachments/assets/5464034f-d102-4bb8-88b3-199b86309805)

**Connected to server Successfully**

![image](https://github.com/user-attachments/assets/70ff0262-cad5-4f89-b930-9adef82bd31c)

### 4.1 Configure Prometheus on ubuntu server
After connecting with server download and Install prometheus on your server. In my case I successfully donaloaded and Installed on my server, If you struggle to do so here is the step-by-step guide to [configure prometheus](https://medium.com/@teddy2000/installation-and-configuration-guide-for-prometheus-and-prometheus-node-exporter-03d16321ec4f)

![image](https://github.com/user-attachments/assets/f4f4634b-7bb9-4a6c-bc29-c3530a510f52)

**Access Prometheus Web Interface**
Prometheus runs on port 9090 by default so you need to allow port 9090 on your Server, With Prometheus running successfully, you can access it via your web browser using **<ip_address>:9090**

![image](https://github.com/user-attachments/assets/e13b2ef3-0301-4e0b-93bd-ff5b44dd44d2)

### 4. 2 Prometheus Node Exporter Setup
**The Node Exporter** is an agent that gathers system metrics and exposes them in a format which can be ingested by Prometheus. The Node Exporter is a project that is maintained through the Prometheus project. This is a completely optional step and can be skipped if you do not wish to gather system metrics. The following will need to be performed on each server that you wish to monitor system metrics for.

![image](https://github.com/user-attachments/assets/a439b2d7-e518-4262-b108-f0704c665f49)

**Add the node exporter job in prometheus.yml file**

`sudo vim /etc/prometheus/prometheus.yml`

![image](https://github.com/user-attachments/assets/6bfa615d-16ec-41de-aa81-feaeb8cac52c)

Node Exporter Job with port 9200, copy and paste the below code in prometheus.yml file.

```
- job_name: "Node_Exporter"

    static_configs:
      - targets: [localhost:9200"]
```
      
**Start and Reload the service**

```
promtool check config /etc/prometheus/prometheus.yml
curl -X POST http://localhost:9090/-/reload
```

![image](https://github.com/user-attachments/assets/e9557a2e-8476-4d2f-bb92-206db2ba3f23)

Go to the prometheus page, then under status-target you will see Node exporter job

![image](https://github.com/user-attachments/assets/4fa02ef1-f17b-42fc-837e-43516856fc22)

### 4.3 Configure Grafana on Ubuntu Server
Connect to server succesfully

![image](https://github.com/user-attachments/assets/6262b06a-048c-4df7-8bee-8a2273dd52cb)

Following server connection, proceed to download and install Prometheus on your server. In my experience, I've successfully set up the Grafana on my server. For a detailed, step-by-step configuration guide, you can refer to my Medium article titled [Setting Up Grafana on Ubuntu 22.04](https://medium.com/@teddy2000/installing-grafana-on-ubuntu-22-04-a-comprehensive-walkthrough-103fb6fe2112)
Once Grafana service is initiated successfully, you should receive confirmation indicating its active status and smooth operation

![image](https://github.com/user-attachments/assets/34c1f52b-acce-4ae0-9bfe-91d0ca1762cb)

To access the grafana page, copy your virtual machine public IP addess with the port number of **3000**, then click on the data source

![image](https://github.com/user-attachments/assets/ecf910f8-f095-442e-b804-291e84814e17)

Under the data source select prometheus as your data source, then give it a **name** and a **URL** for your prometheus after that click "**save & test**". You should see "Successfully queried the Prometheus API".

![image](https://github.com/user-attachments/assets/670b282b-387e-45e6-ad76-3a63a8a8da18)
![image](https://github.com/user-attachments/assets/4aa54e47-3509-4f03-b7bc-7fb5e3485c21)

![image](https://github.com/user-attachments/assets/dbe948a2-d5d3-4f60-9897-909a51079729)

Under "**Dashboards**" Click on "**new**", then select "**import**".

![image](https://github.com/user-attachments/assets/13379872-7cbc-4135-a403-befb9c991d56)

To get node exporter dashboard, enter number "1860" and click on load. This dashboard is forked from the excellent 1860 node exporter dashboard which gives a lot of detail for CPU, disk and network activity.

![image](https://github.com/user-attachments/assets/2f33909c-f9a2-4601-b73d-9ca39f0d9ac0)

Then selct the prometheus data scource(in this case "prometheus" default and click on "**import**"

![image](https://github.com/user-attachments/assets/2574c7ad-3c28-48cc-a90b-e08567c3ba5d)

You should see the prometheus monitoring page of "node **exporter**" as shown below.

![image](https://github.com/user-attachments/assets/47e64aa3-b2dc-4662-ae7e-fa9c4be248c2)

### 4.4 Integrate Prometheus with Jenkins
Now go to Jenkins and download prometheus plugin

![image](https://github.com/user-attachments/assets/5528ef77-2b33-4b4b-8700-36e9c9394cad)

**Add Jenkins Job In prometheus yml**

`sudo vim /etc/prometheus/prometheus.yml
`

![image](https://github.com/user-attachments/assets/d0daae5f-f80d-4e30-812f-c5c14d8548ab)

Add the following jenkins job in "prometheus.yml" file

![image](https://github.com/user-attachments/assets/f723eea8-eb17-4aef-9fe4-c47604969c97)

**Note**: Always check if your "**prometheus.yml**" file is valid prometheus config file syntax

![image](https://github.com/user-attachments/assets/48e72b65-f7b2-47db-b5c6-13af57b6245c)

![image](https://github.com/user-attachments/assets/8ffcede9-8dd1-46ce-b437-bbd524a12fef)

To get Jenkins Information go to prometheus/status/target

![image](https://github.com/user-attachments/assets/4c03533a-9554-460e-8824-6fc2962ea3c0)

click on import dashboard and enter number 9964 and click on load

![image](https://github.com/user-attachments/assets/8aa253cd-d428-4e81-88fa-2624486702b8)

![image](https://github.com/user-attachments/assets/56ff9d57-acc6-4d48-a724-ebf57ab9e93d)

![image](https://github.com/user-attachments/assets/5a584a9e-f3b9-4373-973f-f7b3c44662d6)

Now you should see detailed over view of jenkins information on grafana dashboard

![image](https://github.com/user-attachments/assets/bcfa3fc8-5380-4ff3-b50f-9e5337e01129)

## Step:- 5 Configure Email In Jenkins
To keep informed about build and deployment statuses, provide immediate feedback to developers, facilitate collaboration, and assist in troubleshooting issues within the Jenkins environment, we need to configure email notifications with Jenkins

### 5.1 Download the email plugin in jenkins

![image](https://github.com/user-attachments/assets/b109f223-4b03-4f0a-a86e-98281990e6c8)

### 5.2 Configure Email Notification with Jenkins
Once the plugin is downloaded you should integrate it with Jenkins to do this follow these steps: - **Go to Jenkins Dashboard** → Click on "**Manage Jenkins"** from the left-hand side menu → Select "**Configure System** → Scroll down to the "**Extended E-mail Notification section** → **Configure the SMTP server** settings with your email provider's details (SMTP server, port, authentication, etc.) → Click on "**Save**" to apply the changes

![image](https://github.com/user-attachments/assets/50469f55-db4a-43b4-8b55-3535aa89e85c)

## Step: 6 Install Trivvy
configuring and using Trivy in your DevOps project enables you to vulnerability scanning, compliance checks, integration with CI/CD pipelines, risk mitigation, and continuous monitoring. This enhances overall security, compliance, and risk management throughout the software development and deployment lifecycle

Add repository setting to `/etc/apt/sources.list.d`

```
sudo apt-get install wget apt-transport-https gnupg lsb-releasewget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/nullsudo apt-get update
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```
**You should see the following result**

![image](https://github.com/user-attachments/assets/630e3aaf-8c4b-4357-86e2-7cf32a049332)

### To get TMDB api key

![image](https://github.com/user-attachments/assets/ec51e477-898b-4c70-bdbc-c91f87e51485)

## Step: - 7 Create TMDB API key

To create an api key follow the followind steps:- click on **profile** → **settings** → **API** → **API Key**

![image](https://github.com/user-attachments/assets/6f761489-8f98-41e4-a865-0ccc65bf2aa6)

## Step:- 8 Jenkins Job Pipeline

![image](https://github.com/user-attachments/assets/4e698239-444f-4dff-ba89-e3360dfedefe)

**Here it is Our Jenkins Job Pipeline worked successfully**

![image](https://github.com/user-attachments/assets/8bc15cc4-6799-4324-8442-705dca6f07cc)

![image](https://github.com/user-attachments/assets/c2e72c2a-8b2a-4b54-9e46-78d9f472b4c7)

Explore my Netflix clone, complete with a robust CI/CD pipeline powered by Jenkins. I've taken DevOps to the next level by integrating Docker containers for seamless deployment.

But that's not all! I've also implemented top-notch monitoring tools like Prometheus, Node Exporter, and Grafana to ensure everything runs smoothly.

In terms of security and code quality, I've got you covered with SonarQube, Trivy, and OWASP Dependency Check, ensuring your application stays secure and your code remains pristine.

If you're looking to level up your DevOps game, this project serves as a comprehensive guide. Feel free to drop any comments or questions below—I'm here to help!

