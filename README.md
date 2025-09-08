# Jenkins:
<img width="1133" height="464" alt="image" src="https://github.com/user-attachments/assets/ad08f182-315b-4b1d-9f44-9237b68f2bbd" />
<img width="1157" height="506" alt="image" src="https://github.com/user-attachments/assets/9092510a-5d87-4da2-ac0f-e14ae0d2da0c" />


## Installation of jenkins on EC2 Instance (Ubuntu).

Install Jenkins, configure Docker as agent.

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="1820" height="754" alt="image" src="https://github.com/user-attachments/assets/31fa997e-507f-4f07-aca7-574fa61720d8" />


### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

**Install Java:**
```
sudo apt update
sudo apt install openjdk-21-jre
```
<img width="1883" height="348" alt="image" src="https://github.com/user-attachments/assets/e5e4688e-bb53-457f-b770-15dbd6ea99c5" />


**Verify Java is Installed**:
```
java -version
```
<img width="1063" height="115" alt="image" src="https://github.com/user-attachments/assets/f6618c1a-ac06-499c-bbab-e8ea148bb36a" />

**Now, you can proceed with installing Jenkins**:

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image you can just allow Custom TCP port number 8080.

<img width="1836" height="299" alt="image" src="https://github.com/user-attachments/assets/8982e2d5-ecd4-414f-b105-c3744eee54ad" />


### Login to Jenkins using the below URL:

http://ec2-instance-public-ip-address:8080  
Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to get the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password to Jenkins page.
      
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">


## Docker Slave Configuration

Run the below command to Install Docker:
```
sudo apt update
sudo apt install docker.io
```
<img width="1568" height="310" alt="image" src="https://github.com/user-attachments/assets/19d7da36-6bfe-4ccf-be03-e9c22d1bd261" />


### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```
<img width="791" height="114" alt="image" src="https://github.com/user-attachments/assets/c1091fc6-6e90-48b2-922e-09b04254b822" />
<img width="924" height="430" alt="image" src="https://github.com/user-attachments/assets/3d6a8a32-11a9-4b9d-80ec-f7218a49a3c8" />
<img width="892" height="524" alt="image" src="https://github.com/user-attachments/assets/db559bde-1f64-4430-b6e0-7939611d69c5" />


Once you are done with the above steps, it is better to restart Jenkins.

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   - http://<ec2-instance-public-ip:8080/restart

   
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for the Jenkins to be restarted.

## Creating Pipeline-Job:

- Click on Create a job
<img width="894" height="305" alt="image" src="https://github.com/user-attachments/assets/87928c6e-9ea0-47f8-85d5-2e091f166490" />

- Enter Pipeline name, select "Pipeline" and click OK.
<img width="1133" height="830" alt="image" src="https://github.com/user-attachments/assets/675e1a4b-8b0a-4a37-ad60-35f93cbfbc8d" />

- Scroll down, under the definition select "Pipeline script from SCM"
<img width="1291" height="482" alt="image" src="https://github.com/user-attachments/assets/9ffe48ca-d4a9-40db-8e59-bfe7e46bda32" />

- Under SCM, select "Git", copy Github repository URL and edit branch as "main" after checking Github repositary's branch as shown in the image.
<img width="1252" height="648" alt="image" src="https://github.com/user-attachments/assets/dc666f74-8852-4065-83d8-b3dd4a02d1a4" />

- Copy and paste the file name and path from Github as prefix with Jenkinsfile like "My-first-pipeline/Jenkinsfile" and Save it as per screenshot.
<img width="1255" height="220" alt="image" src="https://github.com/user-attachments/assets/38ce94ec-644d-46ae-adae-675a8ac1d1ef" />

- To build pipeline, click on "Build now" and check for console output.
<img width="858" height="486" alt="image" src="https://github.com/user-attachments/assets/462a7816-4db7-41b1-895f-a2ab6b2c94a1" />

- You can see pipeline build is completed, and jenkins stop docker container as shown below.
<img width="1402" height="736" alt="image" src="https://github.com/user-attachments/assets/ad2fd915-da7d-4baf-a639-9c98cbf0e652" />


**By this method, Jenkins master requested a docker to create a conatainer for application by using "Docker Pipeline" plugin we configured. Docker creates a container, as soon as process is done it will terminate the conatainer.** 

<img width="491" height="302" alt="image" src="https://github.com/user-attachments/assets/f5f77bf2-6b7b-465f-94d4-2ef21cb944ea" />


**When there is a request a container is created, this is the approach and this is how saving the cost and time for worker node configuration upgrade.**

<img width="444" height="315" alt="image" src="https://github.com/user-attachments/assets/af8add8a-cb33-448d-9e43-50cd3c43cd10" />


The docker agent configuration is now successful.




