
# DevOps Project Node-ToDo-CICD

This is a Node Todo CICD App made in NodeJs framework. It allows users to easily add, delete, and update events and tasks. With this App, users can manage their tasks and events.


## In this project I have created Continuous Integration and Continous Deployment using Git, Github, Jenkins, Docker and Trigger Jenkins Jobs using Github Webhooks.




## Steps to follow

1. I created an AWS EC2 instance (Ubuntu + t2-micro) and key pair to securely connect to my instance and enabled SSH, HTTP ports in security group

2. Next I opened a terminal using instance public IP in PuTTY and login as ubuntu 

3. Updated a list of packages in Ubuntu's package manager
```bash
  sudo apt update
```

4. Then I installed Java and checked its version
```bash
  sudo apt install openjdk-11-jre
```
```bash
  java -version
```

5. installed Jenkins 
```bash
 curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

6. Enabled and started the Jenkins
```bash
  sudo systemctl enable jenkins
```
 ```bash
  sudo systemctl start jenkins
```

7. Checked Jenkins status
 ```bash
  sudo systemctl status jenkins
```

8. I know that jenkins opened on port 8080 So I updated the EC2 instance's inbound security added rule customTCP, port 8080 and allow anywhereIpv4

9. Then I opened jenkins with  http://publicIP:8080/

10. (A) In jenkins first of all I created my account and installed necessary jenkins plugins. 
        Then in jenkins followed these steps to create a job =  New Item -> type project name (todo-node-app) -> freestyle project -> github project (here paste the source code url which was in your github repository) -> credentials -> SSH in kind, ID = github-jenkins, write the description(this is for jenkins and github integration), username = ubuntu , private key = paste that key from cat id_rsa , then add that credentials -> branches to build = */master -> save


  (B) To generate SSH key go in terminal and run command  
```bash
  ssh-keygen

  cd .ssh
```
  After the key-generation we SSH public key(id_rsa.pub) in github to connect it to server
  Steps = github -> settings -> SSH and GPG keys -> new SSH key -> fill the title(jenkins project) -> paste that public key(cat id_rsa.pub).

11. Then I installed nodejs and packages in terminal
   
```bash
sudo apt install nodejs

sudo apt install npm

npm install

node app.js
```

12. Then I installed Docker in terminal because I want build container of my application.
 
```bash
 sudo apt install docker.io
```
13. Then I know that Docker opened on port 8000 So I updated the EC2 instance's inbound security added rule customTCP, port 8000 and allow anywhereIpv4

14. I want to automate all the proces like if anyone push or change the code in github then jenkins automatically build the job and update the application.
so I setup the GITHUB-WEBHOOK feature.


## GitHub-Webhook

(a) Install GitHub integration plugin in jenkins

-- steps = Manage plugins -> GitHub integration install without restart the jenkins option.

-- Copy the jenkins URL 

(b)  Open GitHub repository -> repository settings -> Webhook -> add Webhooks -> paste that jenkins URL in PayloadUrl (http://publicip:8080/github-webhook/) -> content type = application/json -> just the push event -> active -> add webhook

-- Open jenkins -> job -> configure -> build triggers -> githubhook trigger for SCM polling -> save

15. In jenkins open the job(todo-node-app) -> configuration -> build steps -> execute shell -> add build steps -> write docker commands 
( docker build . -t node-app-todo

docker run -d --name node-app-container -p 8000:8000 node-app-todo )  -> save

16. Run Build Now in jenkins 
  
   [ After this build command the jenkins pipeline pull that code from github, build a docker image from that code, make a container from that docker image and run that container ]

Our App was running on port 8000 

17. We can see our NODE-TODO-APP by this URL http://publicip:8000/



![1](https://user-images.githubusercontent.com/95110861/235649755-8019e355-130c-47b1-911b-c2011928bc4c.png)



![2](https://user-images.githubusercontent.com/95110861/235649911-e753acea-02d8-4cf7-a3b3-688a668717a5.png)



![3](https://user-images.githubusercontent.com/95110861/235649983-34e69a4a-92a9-482b-9160-f41a95fa9646.png)
