# CI-CD-pipeline-for-dotnetCoreapp-using-jenkins
## 📌Summary of steps
Step 1: Prepare 2 virtual machines that serve as the Jenkins server and the web server.
Step 2: Integrate GitHub and Jenkins via Webhook, using ngrok to assist in this process.
Step 3: Install Jenkins agent on the node and connect it to the Jenkins server.
Step 4: Service and web server configuration: using systemd and nginx to support this process.
Step 5: Write a Jenkinsfile to define the pipeline.
## 📌Step 1: Prepare 2 virtual machines that serve as the Jenkins server and the web server.
### Install Jenkins on the Linux operating system
```
#!/bin/bash
sudo apt install openjdk-17-jdk -y
java --version
wget -p -O - https://pkg.jenkins.io/debian/jenkins.io.key | apt-key add -
sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
apt-get update
apt install jenkins -y
systemctl start jenkins
ufw allow 8080
```
#### Update /etc/hosts for Jenkins hostname on linux
```
ip-of-jenkins-server jenkins.phanhuy.tech
```
#### Update C:\Windows\System32\drivers\etc\hosts on Window
```
ip-of-jenkins-server jenkins.phanhuy.tech
```
#### Configure Nginx as a reverse proxy for Jenkins.
Create conf.d/jenkins.phanhuy.tech.conf at /etc/nginx
```
server {
listen 80;
server_name jenkins.elroydevops.tech;
location / {
  proxy_pass http://jenkins.phanhuy.tech:8080;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection keep-alive;
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
}
}
```
Result:
![Jenkins Dashboard](jenkins_dashboard.jpg)
### Install .NET SDK on the web server.
```
wget https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb
```
```
sudo dpkg -i packages-microsoft-prod.deb
```
```
sudo apt update
```
```
sudo apt install -y dotnet-sdk-8.0
```
## 📌Step 2: Integrate GitHub and Jenkins via Webhook, using ngrok to assist in this process.
### Install ngrok on Windows.
Visit the official ngrok website: [ngrok.com](https://download.ngrok.com/windows?tab=download)
### Authenticate ngrok.
```
ngrok authtoken YOUR_AUTH_TOKEN
```
### Run ngrok 
```
ngrok http http://jenkins.phanhuy.tech:8080
```
### Result:
![ngrok](ngrok.jpg)
### Use the URL from ngrok to set up a webhook connection between GitHub and Jenkins.
Result:
![Webhook.jpg](Webhook.jpg)
## 📌Step 3: Install Jenkins agent on the node and connect it to the Jenkins server.
