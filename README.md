# Developing An Internal React UI library With Jenkins Pipeline

## Table of Contents
- Description
- DevOps Tools
- Pipeline
- Installation
- Usage
- Contact

### Description
This project to creat react library and make it private and to do that i used jenkins to automate the whole system and push the package to nexus repository to make it private, also i wrote a workflow with github to push the package to NPM organization (i have to Subscribe to make it private) 

### DevOps Tools
- Git
- Jenkins
- Ansible
- Nexus
- NPM
- Github Ations

### Pipeline
<div>
  <img src="https://github.com/user-attachments/assets/13dd1818-3f83-41a0-9aa9-56c932ee1ee0" width="1000">
</div>

### Installation
you have to clone this repo:
```
git clone https://github.com/Tarek-Youns/developing-an-internal-React-UI-library.git
```
when you push this code to your repo the workflow will be triggered(if you have a public ip and used webhook) or you can run it manually

### Usage
Follow these steps to use the CI/CD pipeline:

1-Configure Jenkins: Set up Jenkins with the required plugins and configurations.

2-Configure Nexus Repository: Set up Nexus Repository Manager and configure the repository, Before that you can use the ansible role it this repo 
Frist configur your play book like that 
```
- hosts: servers #your hosts
  remote_user: tarek  # choice user to run the commands in the remote machine 
  become: true  # to give sudo to the user 
  roles:
     - install   #role name 
```
second run this command 
```
ansible-playbook playbook.yml -i inventory --ask-become-pass
```
3-Set Jenkins Credentials: Add credentials for GitHub, Nexus Repository, and SonarQube to Jenkins.

4-Create Jenkins Pipeline: Create a new pipeline in Jenkins and paste the provided Jenkinsfile.
Set up Webhook: In your GitHub repository settings, navigate to "Webhooks" and add a new webhook pointing to your Jenkins server's URL. Configure the webhook to trigger on code push events. Ensure that the Jenkins GitHub plugin is installed and configured to receive webhook notifications(in case you have public ip, if you have not one trigger it manually)
Run Pipeline: Trigger the pipeline manually or set up a webhook for automatic triggering on code changes.


### Contact
- [Linked in](https://www.linkedin.com/in/tarek-youns-23b8821ba/)
-  Gmail:tarekyouns2001@gmail.com



  
 
