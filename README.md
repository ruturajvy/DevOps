# DevOps Project 
## Milestone 1
 This milestone demonstrates the use of the Continuos Intergration tool Jenkins and ansible to automate the setup of a simple pipeline for the following two applications:
- A nodejs web application checkbox.io
- An "enterprise" Java system iTrust
 
 The provisioning and configuring of the jenkins server is done on a remote VM using ansible. The build jobs for both applications are setup automatically on jenkins using the jenkins-job-builder and the build jobs are executed using git hooks that are triggered whenever a push is made to their respective repositories. A simple test script is also run in the end after each build to verify whether the proper functionality of the applications remian intact.
   
 ### Getting Started
 **pre-requisites**: AWS account

- Use `git clone https://github.ncsu.edu/alagava/Project-DevOps.git` to clone our project repository.
 
- Go to the proper directory using `cd Project-DevOps/ansible-srv` and run `baker bake` to create the ansible controller VM (ansible-srv).
- SSH into ansible-srv:
 ```
 baker ssh
 ```
And do the following:

 #### AWS credential configuration 
 You need to set up your AWS security credentials before you are able
 to connect to AWS. You can do this by creating a file named "credentials" at ~/.aws/ 
 and saving the following lines in the file:

    [default]
    aws_access_key_id = <your access key id>
    aws_secret_access_key = <your secret key>
    
 You can set the default region and output format by creating a file named config at ~/.aws/ and saving the following:

    [default]
    region=<your default region>
    output=<your desired output format>
#### Generate SSH key pair to enable GitHub passwordless authentication 
Generate a SSH key pair with the following command
```
ssh-keygen -t rsa -b 4096 -C "GitHub" -f ~/.ssh/git-key
```


### Instructions
#### Step 1: Provisioning a remote VM to run the Jenkins build server and to host checkbox and iTrust2 applications.


 


- Run the `provision_ec2.yml` to create an AWS instance to be run as the remote Jenkins build server. This script will also setup the private key file with restricted permissions and populate the inventory file. This inventory file contains the public IP address of the Jenkins build server and path to the private key file. 

```bash
ansible-playbook /ansible-srv/provision_ec2.yml -i 'localhost'
```
#### Step 2: Setup Jenkins on the remote VM
- Run the `jenkins_init.yml` playbook to install Jenkins on the remote VM
```bash
ansible-playbook /ansible-srv/jenkins.yml -i /ansible-srv/inventory
```

#### Step 3: Create jobs for building checkbox and itrust2 using jenkins job builder

- Run the `create_jobs.yml` playbook to setup build jobs for checkbox.io and itrust2 on the jenkins server using jenkins-job-builder.
```bash
ansible-playbook /ansible-srv/create_jobs.yml -i /ansible-srv/inventory
```
- Run the `build_jobs.yml` playbook to copy the `build_checkbox.yml` and `build_itrust.yml` playbooks that perform the actual build of the checkbox.io and itrust2 applications by running the `build_checkbox.yml` and `build_itrust.yml` playbooks respectively

```bash
ansible-playbook /ansible-srv/build_jobs.yml -i /ansible-srv/inventory
```
#### Step 4: 



#### Step 5: Setup a git hook to trigger a jenkins build by setting a post receive hook
- On the Jenkins server, run the following commands to set up post-receive hooks for checkbox and itrust2


For Checkbox:
```bash
mkdir -p latest_checkbox/production.git/
git init --bare latest_checkbox/production.git/
echo "ansible-playbook build_job_checkbox.yml" > latest_checkbox/production.git/hooks/post-receive
chmod 755 latest_checkbox/production.git/hooks/post-receive
```
For iTrust2:
```bash
mkdir -p latest_itrust/production.git/
git init --bare latest_itrust/production.git/
echo "ansible-playbook build_job_checkbox.yml" > latest_checkbox/production.git/hooks/post-receive
chmod 755 latest_checkbox/production.git/hooks/post-receive
```


 
 
 
 ### Checkbox.io
 
 Obtain a local clone of the application using `git clone https://github.com/cvellai/checkbox.io.git`

 Make a simple change like `touch demo.txt` inside the repo and push it upstream.
 
 You should be able to observe the build getting executed on the Jenkins UI.
 
 After the build is done, go to `server-side/site/` inside the local checkbox repo.
 
 Run `npm test`. 
 
 ### iTrust
